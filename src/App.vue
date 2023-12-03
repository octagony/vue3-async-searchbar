<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
const searchTerm = ref('')
const data = reactive({
  todos: []
})

const changeHandler = async () => {
  const req = await fetch('https://jsonplaceholder.typicode.com/todos')
    .then((resp) => resp.json())
    .then((res) => {
      data.todos = res
    })
}

const computedData = computed(() => {
  return data.todos.filter((item) =>
    item.title.toLowerCase().includes(searchTerm.value.toLowerCase())
  )
})

const showHideList = computed(() => {
  if (computedData.value.at(0)?.title === searchTerm.value) {
    return false
  }
  return true
})
</script>

<template>
  <div class="input__wrapper">
    <label class="input">
      Search
      <input type="search" class="input" v-model="searchTerm" @input="changeHandler" />
    </label>
    {{ computedData.length }}
    <ul class="input__list" :class="{ input__list_active: showHideList }">
      <li
        class="input__item"
        v-for="item in computedData"
        :key="item.id"
        @click="searchTerm = item.title"
      >
        {{ item.title }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
::-webkit-scrollbar {
  width: 12.5px;
}

::-webkit-scrollbar-track {
  box-shadow: inset 0 0 25px grey;
  border-radius: 10px;
}

::-webkit-scrollbar-thumb {
  background: black;
  border-radius: 10px;
}

.input__list {
  max-height: 0;
  display: none;
  overflow: auto;
  margin-top: 1rem;
  transition: max-height 0.3s ease-in-out;
  background: transparent;
  color: black;
  border-radius: 1rem;
  padding: 0.5rem;
  transition: max-height 0.3s ease-in-out;
}

.input__list_active {
  max-height: 250px;
  width: 350px;
  transition: max-height 0.3s ease-in-out;
  display: block;
  max-width: 100%;
  background: lightblue;
}

.input__item {
  max-width: max-content;
  border: 1px solid red;
  margin-block: 0.5rem;
  padding: 0.5rem;
  border-radius: 1rem;
  cursor: pointer;
}
</style>
