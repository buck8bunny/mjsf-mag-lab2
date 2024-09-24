<template>
  <button
    :class="buttonClasses"
    @click="handleClick"
  >
    <span v-if="icon" class="mr-2">
      <i :class="icon"></i>
    </span>
    {{ label }}
  </button>
</template>

<script setup>
import { defineProps, computed, ref } from 'vue';

const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  color: {
    type: String,
    default: 'blue',
    validator: (value) => ['red', 'blue', 'green', 'yellow', 'purple'].includes(value),
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large'].includes(value),
  },
  icon: {
    type: String,
    default: '',
  },
});

// Обчислення класів для кнопки
const buttonClasses = computed(() => {
  const sizeClasses = {
    small: 'px-2 py-1 text-sm',
    medium: 'px-4 py-2 text-base',
    large: 'px-6 py-3 text-lg',
  };

  const colorClasses = {
    red: 'bg-red-500 text-white hover:bg-red-600',
    blue: 'bg-blue-500 text-white hover:bg-blue-600',
    green: 'bg-green-500 text-white hover:bg-green-600',
    yellow: 'bg-yellow-500 text-black hover:bg-yellow-600',
    purple: 'bg-purple-500 text-white hover:bg-purple-600',
  };

  return `${sizeClasses[props.size]} ${colorClasses[props.color]} rounded focus:outline-none focus:ring-2 focus:ring-opacity-50`;
});

// Реактивна змінна для повідомлення
const message = ref('');

const handleClick = () => {
  message.value = 'Кнопка натиснута!';
};
</script>

<style scoped>
</style>
