<!-- src/components/QuestionForm.vue -->
<template>
  <div class="card flex justify-center p-4"  style="padding: 0 !important;">
    <!-- Нативний form -->
    <form @submit.prevent="onSubmit" class="flex flex-col gap-4 w-full"  style="padding: 0 !important;">
      <div class="flex flex-col gap-1">
        <!-- PrimeVue Textarea -->
        <Textarea
          v-model="question"
          rows="5"
          cols="30"
          style="resize: none"
          placeholder="Enter your question..."
        />
      </div>
      <Button type="submit" severity="secondary" label="Submit" />
    </form>
  </div>
</template>


<script setup>
import { ref } from 'vue'
import Textarea from 'primevue/textarea'
import Button from 'primevue/button'
import { useToast } from 'primevue/usetoast'
import FileLoader from './FileLoader.vue'

const question = ref('')
const toast = useToast()

function onSubmit() {
  // Просте підтвердження, що поле не порожнє
  if (!question.value.trim()) {
    console.warn('Спроба відправити порожнє питання');
    toast.add({
      severity: 'warn',
      summary: 'Empty question',
      detail: 'Please enter your question.',
      life: 3000
    });
    return;
  }

  // Логування питання
  console.log('Питання відправлено:', question.value);

  // Тут можна відправити запит на сервер, наприклад fetch/axios
  toast.add({
    severity: 'success',
    summary: 'Question submitted',
    detail: question.value,
    life: 3000
  });

  // Очистити поле
  question.value = '';
}

</script>

<style scoped>
/* Додаткові стилі за потреби */
</style>
