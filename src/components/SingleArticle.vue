<script setup>
import { ref, onBeforeUnmount, onMounted } from 'vue';

const props = defineProps({
  title: String
});

const text = ref([]);
const paragraphs = ref([]);
const show = ref(false);
const image = ref("");
const audio = ref("");
const articleTitle = ref("");
const tags = ref([]);
const scrollY = ref(0);

const changeVisbility = () => {
  scrollY.value = window.scrollY;
  show.value = !show.value;
  
  if (show.value) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = '';
  }
};

const loadImage = async () => {
  try {
    const imageModule = await import(`../assets/images/${props.title}.jpg`);
    return imageModule.default;
  } catch (error) {
    console.error('Error loading image:', error);
    return "";
  }
};

const loadAudio = async () => {
  try {
    const asset = await import(`../assets/audio/${props.title}.mp3`);
    return asset.default;
  } catch {
    return "";
  }
};

const loadText = async () => {
  try {
    const asset = await import(`../assets/articles/${props.title}.txt?raw`);
    return asset.default.split('\n');
  } catch (error) {
    console.error('Error loading asset:', error);
    return [];
  }
};

onMounted(async () => {
  image.value = await loadImage();
  text.value = await loadText();
  paragraphs.value = text.value.slice(3);
  articleTitle.value = text.value.at(0);
  tags.value = text.value.at(1).split(',');
  audio.value = await loadAudio();
});

onBeforeUnmount(() => {
  document.body.style.overflow = '';
});
</script>

<template>
    <div>
        <img v-if="image" :src=image class="main-image" @click = "changeVisbility">
        {{ articleTitle }}
    </div>
    <div v-if="show" class="overlay-container" :style="{ top: scrollY + 'px' }" @click = "changeVisbility">
        <div class="overlay" @click.stop>
            <img v-if="image" :src=image class="overlay-image"> 
            <br>
            <audio v-if="audio" controls :src="audio"></audio>
            <p v-for="(paragraph, index) in paragraphs" :key="index">{{ paragraph }}</p>
        </div>
    </div>
</template>

<style>
.main-image {
    width: 100%;
    cursor: pointer;
}

.overlay-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    cursor: default;
    padding-top: 10px;
    padding-bottom:10px;
}

.overlay-container {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    min-height: 100vh;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 99;
    overflow-y: auto;
    overscroll-behavior-y: none;
}

.overlay {
    padding-inline: 10px;
    position: absolute;
    left:50%;
    top:0;
    transform: translateX(-50%);
    width: 60%;
    height: fit-content;
    background-color: rgb(29, 29, 29);
    color:white;
    z-index: 100;
}
</style>