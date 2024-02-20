<script setup>
import { ref } from 'vue'
const newList = ref('')
const props = defineProps({ itemProp: Object, list: Object })

const resetList = () => {
  newList.value = ''
}
const emit = defineEmits(['listCreated'])

const addNewList = () => {
  // props.itemProp.id
  if (!newList) return
  fetch('http://localhost:3000/shoppinglists/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: newList.value,
      items: []
      // ,
      // updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((newList) => {
      emit('listCreated', newList)
      resetList()
    })
}
</script>

<template>
  <form id="newList" @submit.prevent="addNewList">
    <!-- <h2>{{ newList }}</h2> -->
    <input v-model="newList" type="text" placeholder="New List Title" />
    <button type="submit" :disabled="!newList">Add A New List</button>
  </form>
</template>
