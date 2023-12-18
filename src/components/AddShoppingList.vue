<script setup>
import { ref } from 'vue'
const newListTitle = ref('')
const newListItems = ref([])

const resetNote = () => {
  newListTitle.value = ''
  newListItems.value = []
}
const emit = defineEmits(['list-created'])

const createList = () => {
  if (!newListTitle) return
  fetch('http://localhost:3000/lists/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    items: JSON.stringify({
      title: newListTitle.value,
      items: newListItems.value
    })
  })
    .then((res) => res.json())
    .then((list) => {
      emit('listCreated', list)
      resetNote()
    })
}
</script>

<template>
  <form id="AddList" @submit.prevent="createList">
    <h2>{{ newListTitle }}</h2>
    <label class="title" name="title">{{ newListTitle }}</label>
    <input type="text" id="newListTitle" v-model.trim="newListTitle" />
    <br />
    <!-- <label class="items" name="listItems">{{ newListItems }}</label> -->
    <input type="list" id="newListItems" v-model.trim="newListItems" placeholder="New List Items" />
    <button for="btn" action="submit">Add List</button>
  </form>
</template>
