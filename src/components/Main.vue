<script setup lang="ts">
import AppTextArea from '@/components/AppTextArea.vue';
import AppTextSpeed from '@/components/AppTextSpeed.vue';
import AppTextResult from '@/components/AppTextResult.vue';
import AppLoader from '@/components/AppLoader.vue';

import { ref, onBeforeUnmount } from "vue";

const text = ref('');

const isShowLoader = ref(true);
const isFirstChartEntered = ref(false);
const isShowFinishModal = ref(false);

const fetchApiText = async () => {
  isShowLoader.value = true;
  const response = await fetch(
    "https://baconipsum.com/api/?type=all-meat&paras=1&start-with-lorem=1"
  );
  const data = await response.json();
  text.value = data.join("").replace(/  +/g, " ");;
  isShowLoader.value = false;
}

const changeResetStatus = () => {
  isFirstChartEntered.value = false;
  stopMainInterval();
  correctTapCount.value = 0;
  inсorrectTapCount.value = 0;
  timePassed.value = 0;
}

fetchApiText();

const correctTapCount = ref(0); // кол-во правильно введенных символов
const inсorrectTapCount = ref(0); // кол-во неправильно введенных символов

const incrCorrectTapCount = () => {
  correctTapCount.value++;
};

const incrIncorrectTapCount = () => {
  inсorrectTapCount.value++;
};

const timePassed = ref(0);
let speedInterval;

const startMainInterval = () => {
  speedInterval = setInterval(() => {
    timePassed.value++;
  }, 1000);
};

const stopMainInterval = () => {
  clearInterval(speedInterval);
};

const startInterval = () => {
  startMainInterval();
};

const changeStatus = () => {
  isFirstChartEntered.value = true;
}

onBeforeUnmount(() => {
  clearInterval(speedInterval);
});

</script>

<template>
  <div class="container d-flex justify-content-center mt-5">
    
    <AppLoader v-if="isShowLoader"/>

    <div v-else class="card px-3 py-3 border-0 mb-3" style="max-width: 1200px;">
      <div class="row g-0">
        <div class="col-md-8">

        <AppTextArea
          :text="text"
          :is-first-chart-entered="isFirstChartEntered"
          @change-reset-status="changeResetStatus"
          @incr-correct-tap-count="incrCorrectTapCount"
          @incr-incorrect-tap-count="incrIncorrectTapCount"
          @start-interval="startInterval"
          @change-status="changeStatus"  
        />

        </div>
        <div class="col-md-4">
          <div class="card h-100 border-0">
            <div class="card-body d-flex flex-column justify-content-between h-100 px-3 py-3">

              <div class="card-body__info text-center text-md-start">
                <AppTextSpeed :time-passed="timePassed" :correct-tap-count="correctTapCount" />
                <AppTextResult :inсorrect-tap-count="inсorrectTapCount" :correct-tap-count="correctTapCount" :text="text"/>
              </div>

              <button 
                class="btn btn-link fs-5 fw-bold text-uppercase text-decoration-none text-start text-success text-center text-md-start mt-5 mt-sm-3"
                type="button" 
                @click="fetchApiText">
                <i class="bi bi-arrow-clockwise me-1"></i>Заново
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>