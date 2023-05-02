<script setup lang="ts">
import TheHeader from '@/components/header/TheHeader.vue'
import MainPage from '@/components/MainPage.vue'
import AppModal from '@/components/modal/AppModal.vue';

</script>

<template>
  <div>
    <AppModal/>

    <header>
      <TheHeader />
    </header>

    <main>
      <MainPage />
    </main>
  </div>
</template>

<style>
body {
  background-color: rgb(201, 255, 196) !important;
}
</style>
