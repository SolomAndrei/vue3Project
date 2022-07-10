<template lang="">
    <select  v-model="modelValue" @change="changeOption">
        <option disabled value="">Choose from the list</option>
        <option v-for="option in options" :key="option.value" :value="option.value">{{option.name}}</option>
    </select>
</template>
<script>
export default {
    name: 'MySelect',
    props: {
        modelValue: {
            type: String,

        },
        options: {
            type: Array,
            default: () => []
        }
    },
    methods: {
        changeOption(event) {
            console.log(event.target.value);
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>


<style scoped>
</style>