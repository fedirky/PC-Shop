<!-- src/components/QuestionsComponent.vue -->
<template>
  <div class="card p-4" style="padding: 0 !important;">
    <div class="grid grid-cols-8 gap-4">
      <!-- FileLoader: spans rows 1–3 -->
      <div class="col-span-3">
        <FileLoader />
      </div>

      <!-- QuestionForm: spans rows 4–8 -->
      <div class="col-span-5">
        <QuestionForm />
      </div>
    </div>
  </div>
</template>

<script setup>
import FileLoader from './FileLoader.vue'
import QuestionForm from './QuestionForm.vue'
</script>

<style scoped>
/* no additional styles needed */
</style>
