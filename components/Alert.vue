<template>
  <div>
    <v-alert v-model="alert.status" v-if="alert.correct" type="info" dismissible>
        {{ alert.alertMessage }}
    </v-alert>
    <v-alert v-model="alert.status" v-else-if="alert.inccorect" type="error" dismissible>
        {{ alert.alertMessage }}
    </v-alert>
    </div> 
</template>


<script>
export default {
    props:{
        alert: {
            type: Object,
            required: false,
            default: false,
        }
    } 
}
</script>