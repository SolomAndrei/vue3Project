<template>
    <form @submit.prevent>
        <h4>Create new post</h4>
        <MyInput 
        v-model="post.title"
        class="input"
         type="text" 
         placeholder="Name" />
        <MyInput v-model="post.body" class="input" type="text" placeholder="Discription" />
        <MyButton class="btn" @click="createPost">ADD POST</MyButton>
    </form>
</template>

<script>


export default {
    name: 'PostForm',
    data() {
        return {
            post: {
                title: "",
                body: "",
            }
        };
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit("create", this.post);
            this.post = {
                title: "",
                body: ""
            };
        }
    },
   
}
</script>


<style scoped>
form {
    display: flex;
    flex-direction: column;
}



.btn {
    margin-top: 15px;
    align-self: flex-end;
}
</style>