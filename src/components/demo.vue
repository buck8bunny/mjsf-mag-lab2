<template>
    <div class="button-page">
      <h1 class="text-2xl font-bold mb-4">Налаштування Кнопки</h1>
      
      <div class="mb-4">
        <label class="block mb-2">Текст кнопки:</label>
        <input 
          v-model="buttonLabel" 
          type="text" 
          class="border p-2 rounded"
          @input="clearMessage"  
        />
      </div>
      
      <div class="mb-4">
        <label class="block mb-2">Колір кнопки:</label>
        <select v-model="buttonColor" class="border p-2 rounded" @change="clearMessage">
          <option value="blue">Синій</option>
          <option value="red">Червоний</option>
          <option value="green">Зелений</option>
          <option value="yellow">Жовтий</option>
          <option value="purple">Фіолетовий</option>
        </select>
      </div>
      
      <div class="mb-4">
        <label class="block mb-2">Розмір кнопки:</label>
        <select v-model="buttonSize" class="border p-2 rounded" @change="clearMessage">
          <option value="small">Маленька</option>
          <option value="medium">Середня</option>
          <option value="large">Велика</option>
        </select>
      </div>
  
      <Button 
        :label="buttonLabel" 
        :color="buttonColor" 
        :size="buttonSize" 
        @click="handleClick"
      />
      
      <p v-if="message" class="mt-2 text-green-600">{{ message }}</p>
      
      <h2 class="text-xl font-semibold mt-6 mb-4">Вибір з випадаючого списку</h2>
      
      <!-- Перший приклад: простий вибір -->
      <SearchableDropdown 
        :items="simpleItems" 
        :multiSelect="false" 
        @update:modelValue="handleSimpleSelection" 
      />
      
      <h2 class="text-xl font-semibold mt-6 mb-4">Вибір з випадаючого списку (Кілька)</h2>
      
      <!-- Другий приклад: вибір кількох елементів -->
      <SearchableDropdown 
        :items="multiSelectItems" 
        :multiSelect="true" 
        @update:modelValue="handleMultiSelection" 
      />
  
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import Button from './Button.vue'; 
  import SearchableDropdown from './SearchableDropdown.vue'; 
  
  const buttonLabel = ref('Натисни мене');
  const buttonColor = ref('blue');
  const buttonSize = ref('medium');
  const message = ref('');
  
  const simpleItems = ref([
    { id: 1, name: 'Перший' },
    { id: 2, name: 'Другий' },
    { id: 3, name: 'Третій' },
  ]);
  
  const multiSelectItems = ref([
    { id: 1, name: 'Червоний' },
    { id: 2, name: 'Синій' },
    { id: 3, name: 'Зелений' },
    { id: 4, name: 'Жовтий' },
  ]);
  
  const selectedItems = ref([]); // Змінна для зберігання вибраних елементів
  
  const handleClick = () => {
    message.value = 'Кнопка натиснута!';
  };
  
  // Функція для очищення повідомлення
  const clearMessage = () => {
    message.value = '';
  };
  
  // Обробка вибору з простого списку
  const handleSimpleSelection = (selectedItem) => {
    console.log('Вибраний елемент:', selectedItem);
  };
  
  // Обробка вибору з багатьох
  const handleMultiSelection = (selectedItems) => {
    console.log('Вибрані елементи:', selectedItems);
    selectedItems.value = selectedItems; // Оновлення вибраних елементів
  };
  </script>
  
  <style scoped>
  .button-page {
    padding: 20px;
    max-width: 600px;
    margin: auto;
  }
  </style>
  