<script setup>
    const config = useRuntimeConfig()
    const { data } = await useFetch(`${config.public.strapiUrl}/api/posts`)
    const posts = data.value?.data || []
    const selectedCategory = ref('')

    const filteredPosts = computed(() =>{
        if(!selectedCategory.value) return posts
        return posts.filter(post => post.category === selectedCategory.value)
    })
</script>

<template>
    <div class= "home">
        <h1>Blogs</h1>

        <!--Character dropdown-->
        <select v-model ="selectedCategory">
            <option value="">All Categories</option>
            <option value="Tech">Food</option>
            <option value="Travel">Tech</option>
            <option value="Food">Travel</option>
            <option value="Other">Other</option>
        </select>

        <!--cards-->

        <div class = "posts">
            <div v-for="post in filteredPosts" :key="post.id" class = "postCard">
                <h2>{{ post.title }}</h2>
                <p class="author">By {{ post.author }}</p>
                <p class="snippet">{{ post.snippet }}</p>
                <NuxtLink :to="`/posts/${post.documentId}`">Read more...</NuxtLink>
            </div>

            <p v-if="filteredPosts.length === 0">No posts found.</p>
        </div>
    </div>
</template>

<style scoped>

    .home {
    max-width: 1100px;
    margin: 0 auto;
    padding: 3rem 2rem;
    font-family: 'Segoe UI', sans-serif;
    }

    h1 {
    font-size: 2.5rem;
    font-weight: 700;
    color: #1a1a1a;
    margin-bottom: 0.25rem;
    }

    select {
    margin: 1.5rem 0 2.5rem;
    padding: 1px 20px;
    border: 1px solid #ddd;
    border-radius: 999px;
    font-size: 0.9rem;
    background: #fff;
    cursor: pointer;
    outline: none;
    transition: border-color 0.2s;
    }

    select:focus {
    border-color: #534AB7;
    }

    .posts {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    }

    .postCard {
    background: #fff;
    border: 1px solid #eee;
    border-radius: 16px;
    padding: 1.5rem;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    transition: box-shadow 0.2s, transform 0.2s;
    }

    .postCard:hover {
    box-shadow: 0 8px 24px rgba(83, 74, 183, 0.1);
    transform: translateY(-2px);
    }

    .postCard h2 {
    font-size: 1.1rem;
    font-weight: 600;
    color: #1a1a1a;
    }

    .author {
    font-size: 0.85rem;
    color: #888;
    }

    .snippet {
    font-size: 0.95rem;
    color: #555;
    line-height: 1.6;
    flex-grow: 1;
    }

    .postCard a {
    margin-top: 0.75rem;
    font-size: 0.85rem;
    font-weight: 600;
    color: #534AB7;
    text-decoration: none;
    }

    .postCard a:hover {
    text-decoration: underline;
    }

    .posts p {
    color: #999;
    font-size: 0.95rem;
    }
</style>