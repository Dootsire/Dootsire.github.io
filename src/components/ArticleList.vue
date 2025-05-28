<script setup>
import SingleArticle from './SingleArticle.vue'
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const articleList = ref("")

const getArticles = async () => {
  const modules = import.meta.glob('@/assets/articles/*')
  const filePaths = Object.keys(modules)
  const fileNames = filePaths.map(path => path.split('/').pop())
  return fileNames.map(path => path.split('.', 1).pop())
}

const windowWidth = ref(window.innerWidth)

const columnCount = computed(() => {
    if (windowWidth.value <= 540) {
    return 1
  } else if (windowWidth.value <= 864) {
    return 2
  } else {
    return 3
  }
})

const handleResize = () => {
  windowWidth.value = window.innerWidth
}

onMounted(async () => {
  window.addEventListener('resize', handleResize)
  articleList.value = await getArticles();
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>


<template>
  <div class="grid" :style="{ '--column_count': columnCount }">
    <SingleArticle v-for="(article) in articleList" :key="article" :title="article"/>
  </div>
</template>


<style>
.grid {
  display: grid;
  grid-template-columns: repeat(var(--column_count), 1fr);
  grid-gap: 10px;
  padding: 20px;
}
</style>