<script setup lang="ts">
import { toRefs, computed } from "vue";

const props = defineProps<{
  timePassed: number,
  correctTapCount: number
}>();

const { timePassed, correctTapCount } = toRefs(props);

const speedResult = computed(() => {
  /* 
    Формула скорости:
    (количество символов, которые уже введены верно / (количество секунд, прошедших с момента начала набора)) x 60
  */
  const currentSpeedValue = Math.ceil((correctTapCount.value / timePassed.value) * 60);
  if (isNaN(currentSpeedValue) || !isFinite(currentSpeedValue)) {
    return 0;
  } else {
    return currentSpeedValue;
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