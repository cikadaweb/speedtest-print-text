<script setup lang="ts">
import { ref, onBeforeUnmount, watch, toRefs, computed } from "vue";

const props = defineProps<{
  text: string,
  isResetCounter: boolean
}>()

const emit = defineEmits(['changeResetStatus']);

const { text, isResetCounter } = toRefs(props);

let masSymbols = ref([]);

const userInput = ref('');
const lastCharIndex = ref(0);
const iRememberLastError = ref(-1);

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
})

const isCurrent = (index: number) => {
  return index === lastCharIndex.value
}

const isCorrect = (index: number) => {
  return index < lastCharIndex.value
}

const isIncorrect = (index: number) => {
  return index === iRememberLastError.value
}

watch(text, (newValue) => {
  masSymbols = newValue.replace(/  +/g, " ").split('')
});

watch(userInput, (newValue) => {
  const inputLastChar = newValue.charAt(newValue.length - 1);
  const rightLastChar = text.value[lastCharIndex.value];
  console.log('Последний символ введенный пользователем: ', inputLastChar);
  console.log('Последний символ, который нужно ввести: ', rightLastChar);
  if (inputLastChar === rightLastChar) {
    lastCharIndex.value++;
    iRememberLastError.value = -1; // индекс ошибки обнуляем
  } else {
    iRememberLastError.value = lastCharIndex.value;
    console.log('В символе с индексом ', lastCharIndex.value, 'допущена ошибка!')
  }
});

watch(isResetCounter, (newValue) => {
  if (newValue) {
    userInput.value = '';
    lastCharIndex.value = 0;
    iRememberLastError.value = -1;
    emit('changeResetStatus'); // ToDo
  }
}, { immediate: false });

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

.text-area__span {}

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