<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

const dataSlides = ref([]);
let slideWidth = ref(340);
let test = ref(0)

let count = ref(0);

const transformData = (data) => {
  return data.map(item => ({
    ...item,
    isChecked: false
  }))
}

const fetchData = async () => {
  try {
    const response = await fetch('https://picsum.photos/v2/list?page=2&limit=5');
    const jsonData = await response.json();
    dataSlides.value = transformData(jsonData);

  } catch (error) {
    console.error('error', error);
  }
};

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
    test.value = count.value * slideWidth.value;
  }
};

const getImageUrlList = computed(() => dataSlides.value
  .filter(item => item.isChecked)
  .map(item => (
    item.download_url
  ))
)

const totalSlides = computed(() => dataSlides.value.length);
console.log(totalSlides, 'totalSlides');

onMounted(() => {
  fetchData();
});

</script>

<template>
  <section class="section-slider">
    <div class="container">
      <div class="slider-wrapper" :style="`transform: translate(-${test}px);`">
        <div
          v-for="(slide, index) in dataSlides"
          :key="slide.id"
          class="slide"
        >
          <input type="checkbox" class="checkbox-slide" :id="slide.id" v-model="slide.isChecked" />
          <label :for="slide.id" class="heckbox-slide-label">
            <img :src="slide.download_url" alt="" :class="{ 'active': count === index}">
          </label>
        </div>
      </div>
      <div class="btn-holder">
        <button class="btn-slider" @click="prevSlide">Prev</button>
        <button class="btn-slider" @click="nextSlide">Next</button>
      </div>
      <div class="slider-detail-block">
        <div class="table-url" v-if="getImageUrlList.length">
          <div class="table-url__cell" v-for="(imageUrl, index) in getImageUrlList" :key="index">
            {{ imageUrl }}
          </div>
        </div>
        <div v-else class="empty-table">Please choose image</div>
      </div>
    </div>
  </section>
</template>

<style scoped>

</style>
