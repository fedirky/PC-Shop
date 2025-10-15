<!-- src/components/UniversalSelector.vue -->
<template>
  <div class="card flex justify-center w-full">
    <PanelMenu
      :key="menuKey"
      :model="menuModel"
      class="w-full"
      ref="panelMenuRef"
    />
  </div>
</template>

<script setup>
import { ref, computed, nextTick } from 'vue'
import PanelMenu from 'primevue/panelmenu'

// Props:
// - defaultLabel: початковий текст на кореневому пункті, напр. 'Select CPU'
// - prefix: префікс для оновленого тексту після вибору, напр. 'CPU'
// - icon: іконка для кореневого пункту (рядок) (опціонально)
// - options: масив об’єктів для побудови меню
//
// Структура options:
// [
//   {
//     label: 'Intel',
//     icon: 'pi pi-fw pi-microchip',         // іконка для групи
//     children: [                            // або items, але тут використовуємо children
//       { label: 'Core i9-13900K', value: 'Core i9-13900K', icon: 'pi pi-fw pi-angle-right' },
//       ...
//     ]
//   },
//   ...
// ]
//
// Leaf-елементи повинні містити поле value (значення, яке потрапить в подію). Якщо value не вказано, використовується label.
//
// Emits:
// - 'select' з payload = value обраного пункту
const props = defineProps({
  defaultLabel: { type: String, required: true },
  prefix: { type: String, required: true },
  icon: { type: String, default: null },
  options: { type: Array, required: true }
})
const emit = defineEmits(['select'])

const rootLabel = ref(props.defaultLabel)
const menuKey = ref(0)

// Рекурсивна функція для перетворення props.options у формат PanelMenu
function buildItems(arr) {
  return arr.map(item => {
    // Якщо є підпункти
    if (item.children && Array.isArray(item.children)) {
      return {
        label: item.label,
        icon: item.icon,
        items: buildItems(item.children)
      }
    } else {
      // Leaf
      // Використовуємо value, якщо є, або label
      const val = item.value ?? item.label
      return {
        label: item.label,
        icon: item.icon,
        command: () => onSelect(val)
      }
    }
  })
}

// Обробник вибору
function onSelect(value) {
  // Оновлюємо кореневий лейбл: напр. "CPU: Core i9-13900K"
  rootLabel.value = `${props.prefix}: ${value}`
  // Емітимо в батьківський компонент
  emit('select', value)
  // Перезмонтовуємо PanelMenu, щоб воно згорнулося
  // Через nextTick, щоб спочатку оновився rootLabel
  nextTick(() => {
    menuKey.value++
  })
}

// Обчислений модель-об’єкт для PanelMenu
const menuModel = computed(() => {
  const rootItem = {
    label: rootLabel.value,
    icon: props.icon,
    items: buildItems(props.options)
  }
  return [rootItem]
})
</script>

<style scoped>
/* Додаткові стилі за потреби */
</style>
