<template>
  <button class="back-to-top" v-if="showBackToTop" @click="scrollToTop">
    Back to top
  </button>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const scrollTop = ref(0);
let ticking = false;

const scrollToTop = (event) => {
  event.preventDefault();
  window.scrollTo({ top: 0, behavior: "smooth" });
};

const showBackToTop = computed(() => {
  return scrollTop.value > 300;
});

const handleScroll = () => {
  if (!ticking) {
    window.requestAnimationFrame(() => {
      scrollTop.value = window.scrollY;
      ticking = false;
    });
    ticking = true;
  }
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<style scoped>
.back-to-top {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 999;
}

.back-to-top button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
}

.back-to-top button:hover {
  background-color: #0056b3;
}
</style>
