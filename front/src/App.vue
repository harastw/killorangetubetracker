<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const inputValue = ref('');
const savedValue = ref('');
const tor = ref([{ id: 1, name: 'nagato.png' }, { id: 2, name: '011.jpg' }]); // Убедитесь, что имена файлов совпадают с реальными файлами на сервере

const saveValue = () => {
  savedValue.value = inputValue.value;
}

const downloadFile = (fileName: string) => {
  axios({
    url: `http://localhost:8080/download/${fileName}`,
    method: 'GET',
    responseType: 'blob', // Это важно для скачивания файла
  })
  .then((response) => {
    const url = window.URL.createObjectURL(new Blob([response.data]));
    const link = document.createElement('a');
    link.href = url;
    link.setAttribute('download', fileName); // или любой другой название файла
    document.body.appendChild(link);
    link.click();
    link.remove();
  })
  .catch((error) => {
    console.error("Ошибка при скачивании файла:", error);
  });
}
</script>

<template>
  <header>
    <p>Главная Трекер Поиск Группы</p>
  </header>
  <main>
    <h2>{{ savedValue }}</h2>
    <input v-model="inputValue" type="text" placeholder="Введите значение">
    <button @click="saveValue">Сохранить</button>
    <br><br>
    <div class="file-list" v-for="{ id, name } in tor" :key="id">
      <span>{{ name }}</span>
      <button @click="downloadFile(name)">Скачать</button>
    </div>
  </main>
</template>

<style lang="css" scoped>

</style>