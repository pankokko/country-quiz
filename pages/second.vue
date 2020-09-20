<template>
<v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
        <div class="text-center">
            <div>
            <v-alert v-model="alert.status" v-if="alert.correct" type="info" dismissible>
                {{ alert.alertMessage }}
            </v-alert>
              <v-alert v-model="alert.status" v-else-if="alert.inccorect" type="error" dismissible>
                {{ alert.alertMessage }}
            </v-alert>
            </div>
            <v-card class="mx-auto" max-width="344">
                <v-card-title> COUNTRY QUIZ
                </v-card-title>
                <v-card-text>
                    <div class="display-1 text--primary">
                       The population of {{ expectCountry.name }} is...
                    </div>
                    <div v-for='country in countries' :key="country.country_code" class="text--primary">
                          <v-radio-group v-model="picked" :mandatory="false" :disabled="disableRadios">
                            <v-radio :label="String(country.population)"  :value="country.population">
                            </v-radio>
                        </v-radio-group>
                    </div>
                </v-card-text>
                    {{ picked !== false ? `your answer is ${picked}` : '' }}
                <v-card-actions>
                    <v-btn block color="primary" @click="postAnswer(picked)">
                        Answer
                    </v-btn>
                </v-card-actions>
                    <nuxt-link v-if="alert.correct" :to="{ name: 'third', params: {correctCount: (correctNumber)} }">次のページへ移動する</nuxt-link>
            </v-card>
        
        </div>
    </v-col>
</v-row>
</template>

<script>
import axios from 'axios'

export default {
    async asyncData(params) {
        let response = await axios.get("https://restcountries.eu/rest/v2/all") 
        const { data } = response
        if (data) {
            let countries = []
            for (let index = 0; index < 4; index++) {
                const arrayIndex = Math.floor(Math.random() * 200);  
                countries.push({
                    name: data[arrayIndex].name,
                    country_code: data[arrayIndex].numericCode,
                    population: data[arrayIndex].population
                })
            }
            return {
                countries
            }
        }
    },
    data() {
        return {
            picked: false,
            disabledStatus: false,
            correctNum: this.$route.params.correctCount,
            alert: {
                status: false,
                correct: false,
                inccorect: false,
                alertMessage: '',
            },
            expectCountry: {
                name: 'default Country',
                population: 1000,
            },
        }
    },
    computed: {
        correctNumber(){
            if (this.alert.correct) {
                return this.correctNum += 1
            }
        },
        disableRadios(){
            if (this.alert.correct) {
                return this.disabledStatus = true
            }
        },
    },
    mounted(){
        this.expectCountry = this.countries[Math.floor(Math.random() * this.countries.length)]
    },
    methods: {
        postAnswer(answer){
            this.alert.status = true
            this.alert.correct = false
            this.alert.inccorect  =  false
            if (answer === this.expectCountry.population) {
                this.alert.correct = true,
                this.alert.alertMessage = 'correct！'
            } else {
                this.alert.inccorect = true,
                this.alert.alertMessage = 'incorrect'
            }
        },
    },
}
</script>
