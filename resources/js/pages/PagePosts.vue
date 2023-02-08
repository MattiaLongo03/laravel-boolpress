<template>
    <div v-if="isLoading">Loading...</div>
    <div v-else>
        <h1>Index dei post</h1>
        <div v-if="results">
            <div class="row g-3">
                <div v-for="post in results.data" :key="post.id" class="col-sm-6 col-md-4">
                    <div class="card h-100">
                        <img :src="'/storage/' + post.uploaded_img" class="card-img-top" :alt="post.title">
                        <div class="card-body d-flex flex-column">
                            <h5 class="card-title">{{ post.title }}</h5>
                            <p class="card-text flex-grow-1">{{ post.excerpt }}</p>
                            <router-link :to="{name: 'postsShow', params: {slug: post.slug}}" class="btn btn-primary">Leggi</router-link>
                            <a :href="/posts/ + post.slug" class="btn btn-danger">Leggi da link (sbagliato)</a>
                        </div>
                    </div>
                </div>
            </div>

            <nav class="mt-4">
                <ul class="pagination">
                    <li
                        class="page-item"
                        :class="{disabled: results.current_page == 1}"
                    >
                        <a class="page-link" href="" @click.prevent="changePage(results.current_page - 1)">Previous</a>
                    </li>

                    <li
                        v-for="page in results.last_page"
                        :key="page"
                        class="page-item"
                        :class="{active: results.current_page == page}"
                    >
                        <a class="page-link" href="" @click.prevent="changePage(page)">{{ page }}</a>
                    </li>


                    <li
                        class="page-item"
                        :class="{disabled: results.current_page == results.last_page}"
                    >
                        <a class="page-link" href="" @click.prevent="changePage(results.current_page + 1)">Next</a>
                    </li>
                </ul>
            </nav>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isLoading: true,
            results: null,
        }
    },
    methods: {
        changePage(page) {
            this.isLoading = true;
            axios.get('/api/posts?page=' + page)
                .then(response => {
                    this.results = response.data.results;
                    this.isLoading = false;
                    // scrollTo(0, 0);
                });
        }
    },
    created() {
        this.changePage(1);
    }
}
</script>

<style>

</style>
