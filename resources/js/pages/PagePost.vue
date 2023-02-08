<template>
    <Page404 v-if="is404" />
    <div v-else-if="objPost">
        <h1>{{ objPost.title }}</h1>
        <h2>Nella categoria: {{ objPost.category.name }}</h2>
        <div class="tags">
            <span v-for="tag in objPost.tags" :key="tag.id" class="tag">{{ tag.name }}</span>
        </div>
        <img :src="'/storage/' + objPost.uploaded_img" :alt="objPost.title">
        <p>
            {{ objPost.content }}
        </p>
    </div>
    <div v-else>Loading...</div>
</template>

<script>
import Page404 from './Page404';

// TODO: gestire la 404 dei post non esistenti
export default {
    components: {
        Page404,
    },
    props: [
        'slug',
    ],
    data() {
        return {
            is404: false,
            objPost: null,
        }
    },
    created() {
        axios.get('/api/posts/' + this.slug)
            .then(response => {
                if (response.data.success) {
                    this.objPost = response.data.results;
                } else {
                    this.is404 = true;
                }
            });
    }
}
</script>

<style lang="scss" scoped>
    .tag {
        display: inline-block;
        margin: .3em;
        padding: .4em .6em;
        border-radius: 10em;
        background-color: salmon;
    }
</style>
