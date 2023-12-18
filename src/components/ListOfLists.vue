<script setup>
import { ref } from 'vue'
// import NewListForm from './NewListForm.vue'

const lists = ref([])
fetch('http://localhost:3000/lists/', {
  method: 'GET',
  headers: { 'Content-Type': 'application/json' }
})
  .then((res) => res.json())
  .then((data) => (lists.value = data))

const addListToLists = (list) => {
  lists.value = [...lists.value, list]
}
</script>

<template>
  <!-- <NewListForm @list-created="addListToLists" /> -->
  <form id="NewListForm" @submit.prevent="createList">
    <button type="submit">Add New List</button>
  </form>
  <div class="ListOfLists">
    <h2>All Lists</h2>
  </div>
  <div>
    <ul>
      <li v-for="list in lists" :key="list.id">
        {{ list.title }}
        {{ list.items.length }}
      </li>
    </ul>
  </div>
</template>
