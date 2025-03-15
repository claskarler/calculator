<script setup>
import { ref, watch, onMounted } from 'vue';

const expression = ref('');
const theme = ref(localStorage.getItem('theme') || '');

watch(theme, (newTheme) => {
  localStorage.setItem('theme', newTheme);
});

const setTheme = (newTheme) => {
  theme.value = newTheme;
  localStorage.setItem('theme', newTheme);
  document.body.className = newTheme;
};

function handleButtonClick(e) {
  const value = e.target.dataset.value;
  calculateExpression(value);
}

function calculateExpression(value) {
  if (value === 'C') {
    expression.value = '';
  } else if (value === '=' || value === 'Enter') {
    try {
      expression.value = Function('"use strict"; return (' + expression.value + ')')();
    } catch {
      expression.value = 'Error';
    }
  } else if (value === 'Backspace') {
    expression.value = expression.value.slice(0, -1);
  } else {
    expression.value += value;
  }
}

onMounted(() => {
  document.body.className = theme.value;
});
</script>

<template>
  <div :class="theme" class="calculator-container py-4">
    <div class="result-container d-flex justify-content-between align-items-center p-3 rounded border mb-3">
      <div class="dropdown">
        <button
          class="dropdown-toggle"
          type="button"
          id="dropdownMenuButton"
          data-bs-toggle="dropdown"
          aria-expanded="false"
        >
          <span v-if="theme == ''">🧊</span>
          <span v-if="theme == 'melon-theme'">🍈</span>
          <span v-if="theme == 'cherry-theme'">🍒</span>
          <span v-if="theme == 'orange-theme'">🍊</span>
        </button>
        <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <li><a class="dropdown-item" href="#" @click.prevent="setTheme('')">Ice soda</a></li>
          <li><a class="dropdown-item" href="#" @click.prevent="setTheme('melon-theme')">Melon cream soda</a></li>
          <li><a class="dropdown-item" href="#" @click.prevent="setTheme('cherry-theme')">Cherry cola</a></li>
          <li><a class="dropdown-item" href="#" @click.prevent="setTheme('orange-theme')">Orange soda</a></li>
        </ul>
      </div>

      <span class="fs-3 fw-bold">{{ expression }}</span>
    </div>

    <div class="row g-2">
      <template v-for="row in [['C', '%', 'Backspace', '/'], ['7', '8', '9', '*'], ['4', '5', '6', '+'], ['1', '2', '3', '-'], ['**', '0', '.', '=']]" :key="row">
        <div class="col-12 d-flex justify-content-between">
          <button v-for="btn in row" :key="btn" :data-value="btn" @click="handleButtonClick" 
        class="flex-grow-1 m-1 py-2">{{ btn === 'Backspace' ? '←' : btn }}</button>


        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
button {
  min-width: 4rem;
}

.result-container {
  position: relative;
}
.result-container {
  overflow: visible;
}

</style>
