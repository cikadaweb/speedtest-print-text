<script setup lang="ts">
import { toRefs, computed } from "vue";

const props = defineProps<{
  inсorrectTapCount: number,
  correctTapCount: number,
  text: string,
}>();

const { inсorrectTapCount, correctTapCount, text } = toRefs(props);

const exactResult = computed(() => {
  if (correctTapCount.value === 0) {
    return 100;
  }
  return ((100 - (inсorrectTapCount.value / text.value.length) * 100)).toFixed(1);
});

</script>

<template>
  <div class="text-result fs-4 fw-bold text-uppercase mt-5 mt-sm-3">
    <div class="opacity-75">
      <i class="bi bi-bullseye"></i> Точность
    </div>
    <div class="mt-2">
      <span class="text-speed__counter fs-3">{{ exactResult }}</span>%
    </div>
  </div>
</template>

<style scoped>

</style>