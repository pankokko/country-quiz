<template>
<v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
        <div class="text-center">
            <div>
            <v-alert v-if="alert.status === 'correct'" type="success">
                {{ alert.alertMessage }}
            </v-alert>
                <v-alert v-else-if="alert.status === 'incorrect'" type="error">
                {{ alert.alertMessage }}
            </v-alert>
            </div>
         
            <v-card class="mx-auto" max-width="344">
                <v-card-title> COUNTRY QUIZ
                </v-card-title>
                <v-card-text>
                    <div class="display-1 text--primary">
                        {{ expectCountry.capital }} is capital of 
                    </div>
                    <div v-for='country in countries' :key="country.country_code" class="text--primary">
                          <v-radio-group v-model="picked" :mandatory="false">
                            <v-radio :label="country.name"  :value="country.name">
                             {{ country.country_code }}
                            </v-radio>
                        </v-radio-group>
                    </div>
                </v-card-text>
                    {{ picked !== false ? picked : 'your Answer is...' }}
                <v-card-actions>
                    <v-btn block color="primary" @click="postAnswer(picked)">
                        Answer
                    </v-btn>
                </v-card-actions>
            </v-card>
        </div>
    </v-col>
</v-row>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import axios from 'axios'

export default {
    components: {
        Logo,
        VuetifyLogo
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
            picked: false,
            alert: {
                status: '',
                alertMessage: '',
            },
            expectCountry: {
                name: 'default Country',
                capital: 'default capital',
                country_code: '0',
                trueAnswer: false,
            },
        }
    },
      mounted(){
        this.expectCountry = this.countries[Math.floor(Math.random() * this.countries.length)]
        console.log(this.expectCountry)
    },
    methods: {
        postAnswer(answer){
            console.log(answer)
            console.log(this.expectCountry)
            if (answer === this.expectCountry.name) {
                this.alert.status = "correct",
                this.alert.alertMessage = 'correct！'
                this.alert.type = 'success'

            } else {
                this.alert.status = "incorrect",
                this.alert.alertMessage = 'incorrect'
                this.alert.type = 'danger'
            }
        },
    },
}
</script>
