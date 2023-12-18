<script setup>
import { ref } from 'vue'
import ShoppingLists from './components/ShoppingLists.vue'
// import AddShoppingList from './components/AddShoppingList.vue'
const lists = ref([])
</script>

<template>
  <div>
    <h1>Shopping List App</h1>
  </div>
  <div>
    <ShoppingLists />
  </div>
</template>

<style scoped></style>
