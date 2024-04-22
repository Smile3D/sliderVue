<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

const dataSlides = ref(null);

const fetchData = async () => {
  try {
    const response = await fetch('https://picsum.photos/v2/list?page=2&limit=5');
    const jsonData = await response.json();
    dataSlides.value = jsonData;

  } catch (error) {
    console.error('Ошибка при получении данных:', error);
  }
};

const currentIndex = ref(0);

let slideWidth = ref(450);
let sliderWidth = ref(null)
let test = ref(0)

let count = ref(0);


const prevSlide = () => {
  
  if (count.value < 0) {
    const lastSlide = dataSlides.value.pop()
    dataSlides.value.unshift(lastSlide)
  } else {
    count.value--;
    test.value = count.value * slideWidth.value;
  }
};

const nextSlide = () => {

  if (count.value) {
    const firstSlide = dataSlides.value.shift()
    dataSlides.value.push(firstSlide)
  } else {
    count.value++;
    console.log(count.value, 'count.value++')
    test.value = count.value * slideWidth.value;
  }
};



const totalSlides = computed(() => dataSlides.value.length);
console.log(totalSlides, 'totalSlides');

onMounted(() => {
  fetchData();
});



</script>

<template>
  <div class="slider">
    <div class="wrapper-slider" :style="`transform: translate(-${test}px);`">
      <template
        v-for="(slide, index) in dataSlides"
        :key="slide.id"
      >
        <img :src="slide.download_url" alt="" :class="{ 'active': count === index}">
      </template>
    </div>
  </div>
  <div class="btn-holder">
    <button class="btn-slider" @click="prevSlide">Prev</button>
    <button class="btn-slider" @click="nextSlide">Next</button>
  </div>
</template>

<style scoped>

</style>
