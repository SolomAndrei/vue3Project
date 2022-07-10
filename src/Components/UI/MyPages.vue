<template lang="">
   <div class="page__wrapper">
            <div v-for="pageNumber in totalPages" :key="pageNumber" class='page' @click="changePage(pageNumber)"
                :class="{ 'current-page': page === pageNumber }">{{ pageNumber }} </div>
        </div>
</template>
<script>
export default {
    name: 'MyPages',
    props: {
        totalPages: {
            type: Object,
            required: true
        },
        page: {
            type: Object,
            required: true
        },

    },
    methods: {
        changePage(pageNumber) {
            this.$emit('changeCurrentPage', pageNumber)
        },

    }
}
</script>
<style scoped>
.page__wrapper {
    display: flex;
    margin-top: 15px;
    cursor: pointer;

}

.page {
    border: 1px solid black;
    padding: 10px;

}

.current-page {
    border: 2px solid teal;
}
</style>