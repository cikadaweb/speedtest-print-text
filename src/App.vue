<script setup lang="ts">
import TheHeader from '@/components/TheHeader.vue'
import Main from '@/components/Main.vue'
import AppModal from '@/components/AppModal.vue';

</script>

<template>
  <div>
    <AppModal/>

    <header>
      <TheHeader />
    </header>

    <main>
      <Main />
    </main>
  </div>
</template>

<style>
body {
  background-color: rgb(201, 255, 196) !important;
}
</style>
