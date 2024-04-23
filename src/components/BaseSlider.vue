<script setup>
import { ref } from 'vue';
const props = defineProps({
  dataSlides: Array
})

let slideWidth = ref(340);
let transformSlideWidth = ref(0)
let count = ref(0);

const prevSlide = () => {
  if (count.value < 0) {
    const lastSlide = props.dataSlides.pop()
    props.dataSlides.unshift(lastSlide)
  } else {
    count.value--;
    transformSlideWidth.value = count.value * slideWidth.value;
  }
};

const nextSlide = () => {
  if (count.value) {
    const firstSlide = props.dataSlides.shift()
    props.dataSlides.push(firstSlide)
  } else {
    count.value++;
    transformSlideWidth.value = count.value * slideWidth.value;
  }
};
</script>

<template>
  <div class="slider-wrapper">
    <div class="slide-wrapper" :style="`transform: translate(-${transformSlideWidth}px);`">
      <div
        v-for="slide in props.dataSlides"
        :key="slide.id"
        class="slide"
      >
        <input
          type="checkbox"
          class="checkbox-slide"
          :id="slide.id"
          v-model="slide.isChecked"
        />
        <label
          :for="slide.id"
          class="checkbox-slide-label"
        >
          <img
            :src="slide.download_url"
            :alt="slide.author"
          >
        </label>
      </div>
    </div>
    <div class="btn-holder">
      <button class="btn-slider" @click="prevSlide">Prev</button>
      <button class="btn-slider" @click="nextSlide">Next</button>
    </div>
  </div>
</template>
