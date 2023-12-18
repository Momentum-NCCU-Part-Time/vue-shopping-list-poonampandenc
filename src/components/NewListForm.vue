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
  <form id="NewListForm" @submit.prevent="createList">
    <h2>New List</h2>
    <div class="title">
      <input
        type="text"
        id="newListTitle"
        v-model.trim="newListTitle"
        placeholder="New List Title"
      />
    </div>
    <br />
    <div class="items">
      <input
        type="text"
        id="newListItems"
        v-model.trim="newListItems"
        placeholder="New List Items"
      />
    </div>
    <button type="submit" :disabled="!newListTitle">Add List</button>
  </form>
</template>
