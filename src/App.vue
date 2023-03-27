<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
<!--  <header>-->

<!--    <div class="">-->
<!--&lt;!&ndash;      <nav>&ndash;&gt;-->
<!--&lt;!&ndash;        <RouterLink to="/">Home</RouterLink>&ndash;&gt;-->
<!--&lt;!&ndash;        <RouterLink to="/about">About</RouterLink>&ndash;&gt;-->
<!--&lt;!&ndash;      </nav>&ndash;&gt;-->
<!--    </div>-->
<!--  </header>-->

  <RouterView />
</template>

<style scoped>
@font-face {
  font-family: "Monsterrat";
  font-weight: 400;
  font-style: normal;
  font-display: auto;
  unicode-range: U+000-5FF;
  src: local("Monsterrat"), url("./assets/fonts/Montserrat-Regular.ttf") format("truetype");
}
@font-face {
  font-family: "Monsterrat";
  font-weight: 500;
  font-style: normal;
  font-display: auto;
  unicode-range: U+000-5FF;
  src: local("Monsterrat"), url("./assets/fonts/Montserrat-Medium.ttf") format("truetype");
}
@font-face {
  font-family: "Monsterrat";
  font-weight: 600;
  font-style: normal;
  font-display: auto;
  unicode-range: U+000-5FF;
  src: local("Monsterrat"), url("./assets/fonts/Montserrat-SemiBold.ttf") format("truetype");
}
@font-face {
  font-family: "Monsterrat";
  font-weight: 700;
  font-style: normal;
  font-display: auto;
  unicode-range: U+000-5FF;
  src: local("Monsterrat"), url("./assets/fonts/Montserrat-Bold.ttf") format("truetype");
}
header {
  line-height: 1.5;
  max-height: 100vh;
}


nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
