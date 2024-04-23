<script setup>
import { ref, computed, onMounted } from 'vue';
import BaseSlider from './components/BaseSlider.vue';
import BaseLoader from './components/BaseLoader.vue';
const dataSlides = ref([]);

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

const getImageUrlList = computed(() => dataSlides.value
  .filter(item => item.isChecked)
  .map(item => (
    item.download_url
  ))
)

onMounted(() => {
  fetchData();
});

</script>

<template>
  <section class="section-slider">
    <div class="container">
      <div class="title-block">
        <h2>Your slider</h2>
        <p>Your slider Your sliderYour sliderYour sliderYour sliderYour sliderYour sliderYour slider</p>
      </div>
      <div v-if="dataSlides.length">
        <BaseSlider :dataSlides="dataSlides" />
      </div>
      <div v-else class="loader-wrapper">
        <BaseLoader />
      </div>
      <div class="slider-detail-block">
        <div class="table" v-if="getImageUrlList.length">
          <div
            v-for="(imageUrl, index) in getImageUrlList"
            :key="index"
            class="table__row"
          >
            {{ imageUrl }}
            <img
              :src="imageUrl"
              class="table__row-image"
            >
          </div>
        </div>
        <div v-else class="empty-table">Please choose image</div>
      </div>
    </div>
  </section>
</template>
