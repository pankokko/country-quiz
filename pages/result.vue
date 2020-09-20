<template>
<v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
        <div class="text-center">
            <v-card class="mx-auto" max-width="344">
                <v-card-title> COUNTRY QUIZ
                </v-card-title>
                <v-card-subtitle>
                    RESULT
                </v-card-subtitle>
                <v-card-text>
                    You corrected {{ correctNum }} answers
                </v-card-text>
            </v-card>
        </div>
    </v-col>
</v-row>
</template>

<script>

export default {
    data() {
        return {
            correctNum: this.$route.params.correctCount,
        }
    },
}
</script>
