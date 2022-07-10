<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>


<script>
export default {
    name: 'MyButton'

}
</script>


<style scoped>
.btn {
    cursor: pointer;
    padding: 10px 15px;
    background: none;
    border: 1px solid teal;

}
</style>