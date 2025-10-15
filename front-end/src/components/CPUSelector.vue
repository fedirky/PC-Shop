<!-- src/components/CPUSelector.vue -->
<template>
  <UniversalSelector
    defaultLabel="Select CPU"
    prefix="CPU"
    icon="pi pi-fw pi-cog"
    :options="cpuOptions"
    @select="onSelect"
  />
</template>

<script setup>
import { defineEmits } from 'vue'
import UniversalSelector from './UniversalSelector.vue'

const emit = defineEmits(['selectCpu'])

const cpuOptions = [
  {
    label: 'Intel',
    icon: 'pi pi-fw pi-microchip',
    children: [
      { label: 'Core i9-13900K', value: 'Core i9-13900K', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Core i7-13700K', value: 'Core i7-13700K', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Core i5-13600K', value: 'Core i5-13600K', icon: 'pi pi-fw pi-angle-right' }
    ]
  },
  {
    label: 'AMD',
    icon: 'pi pi-fw pi-microchip',
    children: [
      { label: 'Ryzen 9 7950X', value: 'Ryzen 9 7950X', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Ryzen 7 7700X', value: 'Ryzen 7 7700X', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Ryzen 5 7600X', value: 'Ryzen 5 7600X', icon: 'pi pi-fw pi-angle-right' }
    ]
  }
]

function onSelect(value) {
  console.log(`Selected CPU: ${value}`)
  emit('selectCpu', value)
}
</script>
