<template>
    <div >
        <h1>Posts page</h1>
        <MyInput v-model='searchQuery' placeholder='Search...' />
        <div class="app__btns">
            <MyButton @click="showDialog">Create Post</MyButton>
            <MySelect class="mySelect" v-model='selectedSort' :options="sortOptions" />
        </div>

        <MyDialog v-model:show="dialogVisible">
            <PostForm @create='createPost' />
        </MyDialog>
        <!-- <MyPages :totalPages='totalPages' :page="page" @changeCurrentPage="changePage" /> -->

        <PostList :posts='sortedAndSearchedPosts' @remove="removePost" v-if='!isPostsLoading' />
        <div v-else>Loading...</div>
        <div ref="observer" ></div>
    </div>
</template>


<script>
import axios from 'axios'
export default {
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                { value: 'title', name: 'by name' },
                { value: 'body', name: 'by discription' },
                { value: 'id', name: 'by id' },
            ],

        };
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id);
        },
        showDialog() {
            this.dialogVisible = true;
        },
        // changePage(pageNumber) {
        //     this.page = pageNumber;

        // },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = response.data;
            }
            catch (e) {
                alert("Error");
            }
            finally {
                this.isPostsLoading = false;
            }
        },
        async loadMorePosts() {
            try {
                this.page += 1;
               
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = [...this.posts, ...response.data]
            }
            catch (e) {
                alert("Error");
            }            
        },


    },
    mounted() {
        this.fetchPosts();

        const options = {
            rootMargin: '0px',
            threshold: 1.0
        }
        const callback = (entries, observer) => {
            if (entries[0].isIntersecting && this.page < this.totalPages) {
                this.loadMorePosts()
            }
        };
        const observer = new IntersectionObserver(callback, options);
        observer.observe(this.$refs.observer)
    },
    computed: {
        sortedPosts() {
            if (this.selectedSort === 'id') {
                return [...this.posts].sort((post1, post2) => post1[this.selectedSort] - post2[this.selectedSort])
            }
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch: {
        // page() {
        //     this.fetchPosts()
        // }

    }

}
</script>


<style>




.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;


}

.mySelect {
    cursor: pointer;
}



</style>