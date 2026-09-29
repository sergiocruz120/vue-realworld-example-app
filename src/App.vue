<template>
  <div id="app">
    <RwvHeader />
    <router-view></router-view>
    <BackToTop />
    <RwvFooter />
  </div>
</template>

<script setup>
import RwvHeader from "@/components/TheHeader";
import RwvFooter from "@/components/TheFooter";
import BackToTop from "@/components/BackToTop";
</script>

<style></style>
