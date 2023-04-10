<script setup lang="ts">
import { BaseCard, BaseInputGroup, BaseButton } from '@/components/ui';
import type { Task } from '@/model/task';
import { useTasksStore } from '@/stores/tasks';
import { useRoute, useRouter } from 'vue-router';

let task: Task;
const store = useTasksStore();

const { uid } = useRoute().params;
const router = useRouter();

const onBackClick = () => router.back();
const onEditClick = () => router.push(`/edit/${uid}`);
const onDeleteClick = () => {
  store.deleteTask(uid as string);
  router.back();
};

const formatter = Intl.DateTimeFormat('ru', {
  day: '2-digit',
  month: 'long',
  year: 'numeric',
  hour: '2-digit',
  minute: '2-digit'
});

task = store.getTaskByUid(uid as string);
</script>

<template>
  <div class="button-row">
    <base-button @click="onBackClick">Назад</base-button>
    <div class="action-buttons">
      <base-button variant="primary" @click="onEditClick">Редактировать</base-button>
      <base-button variant="error" @click="onDeleteClick">Удалить</base-button>
    </div>
  </div>
  <base-card class="task-card">
    <base-input-group heading="Название">
      <p>{{ task.title }}</p>
    </base-input-group>
    <base-input-group heading="Дата создания">
      <p>{{ formatter.format(Date.parse(task.createdAt)) }}</p>
    </base-input-group>
    <base-input-group heading="Приоритет">
      <p>{{ task.priority }}</p>
    </base-input-group>
    <base-input-group heading="Отметки">
      <p>{{ task.tags.join(',') }}</p>
    </base-input-group>
    <base-input-group heading="Описание" v-if="task.description">
      <p>{{ task.description }}</p>
    </base-input-group>
  </base-card>
</template>

<style scoped lang="scss">
p {
  font-size: 1.1rem;
}
.task-card {
  @include stack;
  gap: 2rem;
  margin-top: 1rem;
}

.button-row {
  @include row;
  justify-content: space-between;
  
  .action-buttons {
    @include row;
  }
}
</style>