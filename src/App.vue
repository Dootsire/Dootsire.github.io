<script setup>
import ArticleList from './components/ArticleList.vue'
</script>

<template>
  <body>
    <div class="container">
      <p style="font-size: 30px;"> Piotr Fedczuk </p>
      <p>I'm a data and audio journalist based in Chicago. Currently, I work as an investigative reporter 
        for CU-CitizenAccess.</p>
      <p>In the past, I've reported for Illinois Public Media and The Daily Illini. At these news organizations, 
        I've published breaking news, event coverage and scientific articles. </p>
      <p>You can view my work below. Click on an article to read or listen to it, and click out of it to exit.</p>
      <ArticleList />
    </div>
  </body>
</template>

<style scoped>
  .container {
    width: 70%;
    padding: 20px;
    margin-inline: auto;
    font-size: 20px;
  }
</style>