<script setup>
import { defineProps, defineEmits } from 'vue';

// Приймаємо пропси
const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  color: {
    type: String,
    default: 'blue',
  },
  size: {
    type: String,
    default: 'medium',
  },
  icon: {
    type: String,
    default: null,
  },
});

// Обробка події натискання
const emit = defineEmits(['click']);

// Мапінг розмірів на класи Tailwind
const sizeClasses = {
  small: 'py-1 px-3 text-sm',
  medium: 'py-2 px-4 text-base',
  large: 'py-3 px-6 text-lg',
};

// Мапінг кольорів на класи Tailwind
const colorClasses = {
  red: 'bg-red-500 hover:bg-red-600 text-white',
  blue: 'bg-blue-500 hover:bg-blue-600 text-white',
  green: 'bg-green-500 hover:bg-green-600 text-white',
};
</script>

<template>
  <button
    :class="`flex items-center justify-center rounded ${sizeClasses[size]} ${colorClasses[color]} focus:outline-none focus:ring-2 focus:ring-offset-2`"
    @click="$emit('click')"
  >
    <!-- Якщо є іконка, виводимо її -->
    <span v-if="icon" :class="`mr-2 material-icons`">{{ icon }}</span>
    {{ label }}
  </button>
</template>

<style scoped>
.material-icons {
  font-size: 1.25em;
}
</style>
