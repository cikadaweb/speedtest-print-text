<script setup lang="ts">
import { toRefs, computed } from "vue";

const props = defineProps<{
  timePassed: number,
  correctTapCount: number
}>();

const { timePassed, correctTapCount } = toRefs(props);

const speedResult = computed(() => {
  if (Math.ceil((correctTapCount.value / timePassed.value) * 60).toString() === 'NaN') {
    return 0;
  } else {
    return Math.ceil((correctTapCount.value / timePassed.value) * 60);
  }
});
</script>

<template>
  <div class="text-speed fs-4 fw-bold text-uppercase">
    <div class="opacity-75">
      <i class="bi bi-speedometer"></i> Скорость
    </div>
    <div class="mt-2">
      <span class="text-speed__counter fs-3">{{ speedResult }}</span> ЗН./МИН
    </div>
  </div>
</template>

<style scoped>

</style>