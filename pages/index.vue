<template>
<v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
        <div class="text-center">
            <alert :alert="alert"></alert>
            <v-card class="mx-auto" max-width="344">
                <v-card-title> COUNTRY QUIZ
                </v-card-title>
                <v-card-text>
                    <div class="display-1 text--primary">
                        {{ expectCountry.capital }} is capital of 
                    </div>
                    <div v-for='country in countries' :key="country.country_code" class="text--primary">
                          <v-radio-group v-model="picked" :mandatory="false" :disabled="disableRadios">
                            <v-radio :label="country.name" :value="country.name">
                            </v-radio>
                        </v-radio-group>
                    </div>
                </v-card-text>
                    {{ picked !== false ? picked : 'your Answer is...' }}
                    <answer-button :picked="picked" @postEvent="postAnswer"></answer-button>
                    <nuxt-link v-if="alert.correct" :to="{name: 'second', params: {correctCount: (correctNumber)}}">次のページへ移動する</nuxt-link>
            </v-card>
        </div>
    </v-col>
</v-row>
</template>

<script>
import alert from '~/components/Alert.vue'
import AnswerButton from '~/components/AnswerButton.vue'
import axios from 'axios'
export default {
    components: {
        alert,
        AnswerButton,
    },
    async asyncData(params) {
            let response = await axios.get("https://restcountries.eu/rest/v2/all") 
            const { data } = response
            if (data) {
                let countries = []
                let expectCountry = {}
                for (let index = 0; index < 4; index++) {
                    const arrayIndex = Math.floor(Math.random() * 200);  
                    countries.push({
                        name: data[arrayIndex].name,
                        capital: data[arrayIndex].capital,
                        country_code: data[arrayIndex].numericCode,
                    })
                }
                return {
                    countries
                }
            }
    },
    data() {
        return {
            picked: '',
            disabledStatus: false,
            correctNum: 0,
            alert: {
                correct: false,
                inccorect: false,
                alertMessage: '',
            },
            expectCountry: {
                name: 'default Country',
                capital: 'default capital',
                country_code: '0',
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
            if (answer === this.expectCountry.name) {
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
