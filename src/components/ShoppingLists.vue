<script setup>
import { ref } from 'vue'
import AddShoppingList from './AddShoppingList.vue'

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
  <AddShoppingList @list-created="addListToLists" />
  <button @click="createList()" type="submit">Add New List</button>

  <div class="Lists">
    <h2>Shopping Lists</h2>
  </div>
  <div>
    <ul>
      <li v-for="list in lists" :key="list.id">
        {{ list.title }}: {{ list.items.length }} items
        <br />
        {{ list.updatedAt }}
      </li>
    </ul>
  </div>
</template>
