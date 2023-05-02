<script setup lang="ts">
import { ref, onBeforeUnmount, watch, toRefs } from "vue";

const props = defineProps<{
  text: string,
  isFirstChartEntered: boolean
}>();

const emits = defineEmits(['change-reset-status', 'incr-correct-tap-count', 'incr-incorrect-tap-count', 'start-interval', 'change-status']);

const { text, isFirstChartEntered } = toRefs(props);

const masSymbols = ref([] as string[]);

const userInput = ref('');
const lastCharIndex = ref(0);
const iRememberLastErrorIndex = ref(-1);


const handleKeyDown = (event: any) => {
  const regex = /^[a-zA-Z0-9\s\p{P}]+$/u; // регулярное выражение для букв, цифр и пробелов
  if (regex.test(event.key)) {
    userInput.value += event.key;
  }
  // console.log('userInput: ', userInput.value)
}

window.addEventListener('keypress', handleKeyDown);

onBeforeUnmount(() => {
  window.removeEventListener('keypress', handleKeyDown);
});

const isCurrent = (index: number) => {  // текщий символ
  return index === lastCharIndex.value;
};

const isCorrect = (index: number) => { // все предыдущии символы корректны
  return index < lastCharIndex.value;
};

const isIncorrect = (index: number) => { // если данный индекс уже был ошибкой
  return index === iRememberLastErrorIndex.value;
};

watch(text, (newValue) => { // обновился текст
  masSymbols.value = newValue.replace(/  +/g, " ").split('');
  userInput.value = '';
  lastCharIndex.value = 0;
  iRememberLastErrorIndex.value = -1;
  emits('change-reset-status');
}, { immediate: true });

watch(userInput, (newValue) => {
  if (!isFirstChartEntered.value) { // проверка на первый ввод
    emits('start-interval');
  }
  emits('change-status'); // меняем флаг запуска интервала

  const inputLastChar = newValue.charAt(newValue.length - 1);
  const correctLastChar = text.value[lastCharIndex.value];

  // console.log('Последний символ введенный пользователем: ', inputLastChar);
  // console.log('Последний символ, который нужно ввести: ', correctLastChar);
  if (inputLastChar === correctLastChar) {
    lastCharIndex.value++;
    iRememberLastErrorIndex.value = -1; // индекс ошибки обнуляем
    emits('incr-correct-tap-count');
  } else {
    if (iRememberLastErrorIndex.value === lastCharIndex.value) {
      return;
    }
    iRememberLastErrorIndex.value = lastCharIndex.value;
    emits('incr-incorrect-tap-count');
    // console.log('В символе с индексом ', lastCharIndex.value, 'допущена ошибка!');
  }
});

</script>

<template>
  <div class="text-area card h-100 border-0">
    <div class="text-area__body card-body">
      <input class="text-area__input" type="text" v-model="userInput">
      <span class="text-area__span fs-5 d-inline" :class="[
          { bggreen: isCurrent(index) },
          { 'passed-text': isCorrect(index) },
          { bgred: isIncorrect(index) }
        ]" v-for="(char, index) in masSymbols" :key="index">
        {{ char }}
      </span>
    </div>
  </div>
</template>

<style scoped>
.text-area__body {
  position: relative;
}

.text-area__input {
  position: absolute;
  left: 0;
  top: -10px;
  width: 100%;
  /* height: 1px;
  font-size: 16px;
  overflow: hidden;
  border: none;
  color: transparent;
  background-color: transparent;
  caret-color: transparent;
  outline: 0; */
}

.passed-text {
  color: #5bc538;
}

.bggreen,
.bgred {
  padding: 3px;
  border-radius: 3px;
  color: #fff;
}

.bggreen {
  background-color: #5bc538;
}

.bgred {
  background-color: #F36747;
}
</style>