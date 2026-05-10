<script setup>
    import { marked } from 'marked'

    const config = useRuntimeConfig()
    const route = useRoute()
    const id = route.params.id

    const { data, pending, error} = await useFetch(`${config.public.strapiUrl}/api/posts/${id}`)

    const post = computed(() => data.value?.data || null)

    const renderedContent = computed(() => {
        if(!post.value?.content) return ''
        return marked(post.value.content)
    })
</script>

<template>
    <div class="postPage">
        <p v-if="pending">Loading post...</p>
        <p v-else-if="error">Post not found</p>

        <article v-else-if="post">
            <h1>{{ post.title }}</h1>
            <p class="author">By {{ post.author }}</p>
            <p class="category">Category: {{ post.category }}</p>
            <hr />
     
            <div class="content" v-html="renderedContent"></div>
        </article>

        <NuxtLink to="/">Back to Home</NuxtLink>
    </div>
</template>


<style scoped>

    .postPage {
        max-width: 720px;
        margin: 0 auto;
        padding: 50px 30px;
        font-family: 'Segoe UI', sans-serif;
    }

    article {
        margin-bottom: 25px;
    }

    h1 {
        font-size: 30px;
        font-weight: 700;
        color: #1a1a1a;
        line-height: 1.3;
        margin-bottom: 10px;
    }

    .author {
        font-size: 15px;
        color: #888;
        margin-bottom:5px;
    }

    .category {
        display: inline-block;
        font-size: 14px;
        font-weight: 600;
        color: #534AB7;
        background: #EEEDFE;
        padding: 3px 12px;
        border-radius: 30px;
        margin-bottom: 15px;
    }

    hr {
        border: none;
        border-top: 2px solid #f0f0f0;
        margin: 24px 0 30px;
    }

    :deep(.content) {
        font-size: 16px;
        line-height: 1.9;
        color: #333;
    }

    :deep(.content h2) {
        font-size: 20px;
        font-weight: 600;
        margin: 30px 0 10px;
        color: #1a1a1a;
    }

    :deep(.content h3) {
        font-size: 25px;
        font-weight: 600;
        margin: 25px 0 4px;
        color: #1a1a1a;
    }

    :deep(.content p) {
        margin-bottom: 20px;
    }

    :deep(.content ul),:deep(.content ol) {
        padding-left: 25px;
        margin-bottom: 20px;
    }

    :deep(.content li) {
        margin-bottom: 0.4rem;
    }

    :deep(.content code) {
        background: #f5f5f5;
        padding: 2px 7px;
        border-radius: 4px;
        font-size: 0.88rem;
        color: #d63384;
    }

    :deep(.content blockquote) {
        border-left: 4px solid #534AB7;
        padding: 0.5rem 1rem;
        margin: 1.5rem 0;
        color: #555;
        background: #EEEDFE;
        
    }

    .postPage > p {
        color: #999;
        font-size: 0.95rem;
        padding: 1rem 0;
    }

    a {
        display: inline-block;
        font-size: 0.9rem;
        font-weight: 600;
        color: #534AB7;
        text-decoration: none;
    }

    a:hover {
        text-decoration: underline;
    }

</style>
