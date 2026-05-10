<script setup>
    const config = useRuntimeConfig()
    const { data} = await useFetch(`${config.public.strapiUrl}/api/posts`)

    const posts = data.value?.data || []

    const searchQuery = ref('')

    const searchResults = computed(() => {
        if (!searchQuery.value) return []

        const query = searchQuery.value.toLowerCase()

        return posts.filter(post => {
            const titleMatch = post.title.toLowerCase().includes(query)
            const authorMatch = post.author.toLowerCase().includes(query)
            return titleMatch || authorMatch
        })
    })
</script>

<template>
    <div class = "search">
        <input v-model="searchQuery" type="text" placeholder = "Search by title or author"/>
        
        <div class="results">
            <div 
                v-for="post in searchResults" :key = "post.id" class="post-card">
                <h2>{{ post.title }}</h2>
                <p class="author">By {{ post.author }}</p>
                <p class="snippet">{{ post.snippet }}</p>
                <NuxtLink :to="`/posts/${post.documentId}`">Read more →</NuxtLink>


            </div>

            <p v-if="searchQuery && searchResults.length === 0">No posts found for "{{ searchQuery }}"</p>

            <p v-if="!searchQuery">Start typing to search...</p>
        </div>
    </div>

</template>

<style scoped>

    .search {
        max-width: 1100px;
        margin: 0 auto;
        padding: 3rem 2rem;
        font-family: 'Segoe UI', sans-serif;
    }

    input {
        width: 100%;
        max-width: 500px;
        padding: 10px 23px;
        border: 1px solid #ddd;
        border-radius: 999px;
        font-size: 15px;
        outline: none;
        background: #fff;
        transition: border-color 0.2s;
        display: block;
        margin-bottom: 2.5rem;
    }

    input:focus {
        border-color: #534AB7;
    }

    .results {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
        gap: 1.5rem;
    }

    .post-card {
        background: #fff;
        border: 1px solid #eee;
        border-radius: 16px;
        padding: 1.5rem;
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        transition: box-shadow 0.2s, transform 0.2s;
    }

    .post-card:hover {
        box-shadow: 0 8px 24px rgba(83, 74, 183, 0.1);
        transform: translateY(-2px);
    }

    .post-card h2 {
        font-size: 16px;
        font-weight: 600;
        color: #1a1a1a;
    }

    .author {
        font-size: 14px;
        color: #888;
    }

    .snippet {
        font-size: 15px;
        color: #555;
        line-height: 1.6;
        flex-grow: 1;
    }

    .post-card a {
        margin-top: 12px;
        font-size: 14px;
        font-weight: 600;
        color: #534AB7;
        text-decoration: none;
    }

    .post-card a:hover {
        text-decoration: underline;
    }

    .results p {
        color: #999;
        font-size: 0.95rem;
        padding: 1rem 0;
    }
</style>