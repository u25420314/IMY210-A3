<template>
  <div>
    <nav>

      <div class="links">
        <NuxtLink to="/">Home</NuxtLink>
        <NuxtLink to="/search">Search</NuxtLink>
      </div>
      
    </nav>
    <NuxtPage />
  </div>
</template>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #f9f9f9;
    color: #1a1a1a;
  }

  nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1.2rem 2rem;
    background: #ffffff;
    border-bottom: 1px solid #eee;
    position: sticky;
    top: 0;
    z-index: 100;
  }


  .links {
    display: flex;
    gap: 2rem;
  }

  .links a {
    font-size: 0.95rem;
    color: #555;
    text-decoration: none;
    transition: color 0.2s;
  }

  .links a:hover {
    color: #534AB7;
  }

  .links a.router-link-active {
    color: #534AB7;
    font-weight: 600;
  }
  </style>