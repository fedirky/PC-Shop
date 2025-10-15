<!-- src/components/CASESelector.vue -->
<template>
  <UniversalSelector
    defaultLabel="Select Case"
    prefix="Case"
    icon="pi pi-fw pi-box"
    :options="caseOptions"
    @select="onSelect"
  ></UniversalSelector>
</template>

<script setup>
import { defineEmits, computed } from 'vue'
import UniversalSelector from './UniversalSelector.vue'

// Emit event when a case is selected
const emit = defineEmits(['selectCase'])

// Use Vite import.meta.glob to find all images under each case folder.
// Adjust the glob pattern to match your image extensions.
const caseImageModules = import.meta.glob(
  '../assets/images/cases/*/*.{jpg,jpeg,png,webp}',
  { eager: true, as: 'url' }
)

// Extract unique folder names under 'cases'
const caseFolders = (() => {
  const names = new Set()
  for (const path in caseImageModules) {
    // Match the folder name between 'cases/' and the next '/'
    // E.g. '../assets/images/cases/CaseName/image.jpg'
    const match = path.match(/\/cases\/([^\/]+)\//)
    if (match && match[1]) {
      names.add(match[1])
    }
  }
  return Array.from(names).sort((a, b) =>
    a.localeCompare(b, undefined, { sensitivity: 'base' })
  )
})()

// Build options array for UniversalSelector
const caseOptions = computed(() => {
  return caseFolders.map(folderName => ({
    label: folderName,
    value: folderName,
    icon: 'pi pi-fw pi-angle-right'
  }))
})

function onSelect(value) {
  console.log(`Selected Case: ${value}`)
  emit('selectCase', value)
}
</script>

<style scoped>
/* Optional: additional component-specific styles */
</style>
