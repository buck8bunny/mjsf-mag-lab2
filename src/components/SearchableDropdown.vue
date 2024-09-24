<template>
    <div class="relative w-full" @click="handleClickOutside">
      <input
        type="text"
        class="border rounded p-2 w-full"
        placeholder="Пошук..."
        v-model="searchTerm"
        @focus="showOptions = true"
        @keydown="handleKeydown"
      />
      
      <!-- Відображення вибраних елементів -->
      <div v-if="selectedItems.length > 0" class="mt-1">
        <span class="text-gray-600">Вибрані:</span>
        <span v-for="(item, index) in selectedItems" :key="index" class="bg-blue-100 rounded p-1 mr-1">
          {{ item.name }}
        </span>
      </div>
      
      <div
        v-if="showOptions"
        class="absolute z-10 bg-white border rounded mt-1 w-full shadow-lg max-h-60 overflow-auto"
      >
        <div v-if="filteredItems.length === 0" class="p-2 text-gray-500">
          Нічого не знайдено
        </div>
        <ul>
          <li
            v-for="(item, index) in filteredItems"
            :key="item.id"
            @click="selectItem(item)"
            @mouseenter="hoveredIndex = index"
            :class="{
              'bg-blue-100': hoveredIndex === index,
              'p-2 cursor-pointer': true,
            }"
          >
            <div class="flex items-center">
              <span v-if="item.icon" class="mr-2">
                <i :class="item.icon"></i>
              </span>
              <span>{{ item.name }}</span>
            </div>
          </li>
        </ul>
        <button @click="clearSelection" class="text-red-500 p-2">
          Очистити
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted, onBeforeUnmount } from 'vue';
  import { defineProps, defineEmits } from 'vue';
  
  const props = defineProps({
    items: {
      type: Array,
      required: true,
    },
    multiSelect: {
      type: Boolean,
      default: false,
    },
  });
  
  const emit = defineEmits(['update:modelValue']);
  
  const searchTerm = ref('');
  const showOptions = ref(false);
  const hoveredIndex = ref(-1);
  const selectedItems = ref([]);
  
  const filteredItems = computed(() => {
    return props.items.filter(item => 
      item.name.toLowerCase().includes(searchTerm.value.toLowerCase())
    );
  });
  
  // Функція для закриття списку
  const closeDropdown = () => {
    showOptions.value = false;
  };
  
  // Вибір елемента
  const selectItem = (item) => {
    if (props.multiSelect) {
      if (!selectedItems.value.includes(item)) {
        selectedItems.value.push(item);
      }
    } else {
      selectedItems.value = [item];
      searchTerm.value = item.name; // Оновлення поля вводу вибраним елементом
      closeDropdown(); // Сховати варіанти
    }
    emit('update:modelValue', selectedItems.value);
  };
  
  // Очистити вибір
  const clearSelection = () => {
    selectedItems.value = [];
    searchTerm.value = '';
    emit('update:modelValue', selectedItems.value);
  };
  
  // Обробка натисків клавіш
  const handleKeydown = (event) => {
    if (event.key === 'ArrowDown') {
      hoveredIndex.value = Math.min(hoveredIndex.value + 1, filteredItems.value.length - 1);
    } else if (event.key === 'ArrowUp') {
      hoveredIndex.value = Math.max(hoveredIndex.value - 1, 0);
    } else if (event.key === 'Enter') {
      const selected = filteredItems.value[hoveredIndex.value];
      if (selected) {
        selectItem(selected);
      }
    } else if (event.key === 'Escape') {
      closeDropdown(); // Сховати варіанти на escape
    }
  };
  
  // Обробник натисків миші поза списком
  const handleClickOutside = (event) => {
    const dropdown = event.target.closest('.relative'); // Знайти найближчий елемент з класом .relative
    if (!dropdown) {
      closeDropdown(); // Якщо клік поза списком, закрити його
    }
  };
  
  // Додати слухача подій на клік
  onMounted(() => {
    document.addEventListener('click', handleClickOutside);
  });
  
  onBeforeUnmount(() => {
    document.removeEventListener('click', handleClickOutside);
  });
  </script>
  
  <style scoped>

  </style>
  