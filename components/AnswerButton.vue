<template>
    <div>
    <v-card-actions>
        <v-btn block color="primary" @click="getAnswer(picked)">
            Answer
        </v-btn>
    </v-card-actions>
    </div>
</template>

<script>
export default {
    props: {
        picked: {
            required: true,
        },
    },
    methods: {
        getAnswer(picked){
            this.$emit('postEvent', picked)
        }
    }
}
</script>