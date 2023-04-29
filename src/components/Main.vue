<script setup lang="ts">
import AppTextArea from '@/components/AppTextArea.vue';
import AppTextSpeed from '@/components/AppTextSpeed.vue';
import AppTextResult from '@/components/AppTextResult.vue';
import { ref } from "vue";

const text = ref('');
const isResetCounter = ref(false);

const fetchApiText = async () => {
  const response = await fetch(
    "https://baconipsum.com/api/?type=all-meat&paras=2&start-with-lorem=1"
  );
  const data = await response.json();
  text.value = data.join("");
  isResetCounter.value = true;
}

const changeResetStatus = () => {
  isResetCounter.value = false;
}

fetchApiText();

</script>

<template>
  <div class="container d-flex justify-content-center mt-5">
    <div class="card px-3 py-3 border-0 mb-3" style="max-width: 1200px;">
      <div class="row g-0">
        <div class="col-md-8">

          <AppTextArea :text="text" :isResetCounter="isResetCounter" @changeResetStatus="changeResetStatus" />

        </div>
        <div class="col-md-4">
          <div class="card h-100 border-0">
            <div class="card-body d-flex flex-column justify-content-between h-100 px-3 py-3">

              <div class="card-body__info">
                <AppTextSpeed />
                <AppTextResult />
              </div>

              <button class="btn btn-link fs-5 fw-bold text-uppercase text-decoration-none text-start text-success"
                type="button" @click="fetchApiText"><i class="bi bi-arrow-clockwise"></i> Заново</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
