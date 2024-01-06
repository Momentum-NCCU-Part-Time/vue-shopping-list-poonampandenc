<script setup>
import { ref } from 'vue'
import AddShoppingList from './AddShoppingList.vue'
import ShoppingListItems from './ShoppingListItems.vue'
// import Items from './Items.vue'
import AddItem from './AddItem.vue'

const lists = ref([])
// const purchased = ref(false)
const addItem = ref('')

fetch('http://localhost:3000/lists/', {
  method: 'GET',
  headers: { 'Content-Type': 'application/json' }
})
  .then((res) => res.json())
  .then((data) => (lists.value = data))

const addNewList = (list) => {
  lists.value = [...lists.value, list]
}

const addNewItem = (updatedList) => {
  console.log(lists)
  let idx = lists.value.findIndex((list) => list.id === updatedList.id)
  lists.value[idx] = updatedList
}

const deleteList = (listIdToDelete) => {
  let listidx = lists.value.findIndex((list) => list.id === listIdToDelete)
  lists.value.splice(listidx, 1)
}
</script>

<template>
  <div class="Lists">
    <AddShoppingList @listCreated="addNewList" />

    <h2>Shopping Lists</h2>

    <div v-for="list in lists" :key="list.id">
      <!-- <ul>
      <li v-for="list in lists" :key="list.id">
        {{ list.title }}: {{ list.items.length }} items
        <br />
        {{ list.updatedAt }}
      </li>
    </ul> -->

      <ShoppingListItems :list="list" @listUpdated="addNewItem" @listDeleted="deleteList" />
      <!-- <form class="itemForm" @submit.prevent="addNewItem(list)">
        <input v-model="addNewItem" type="text" placeholder="Add Item" />
        <button type="submit">Add New Item</button>
      </form> -->

      <!-- <AddItem :list="list" @itemCreated="addNewItem" /> -->
    </div>
  </div>
</template>
