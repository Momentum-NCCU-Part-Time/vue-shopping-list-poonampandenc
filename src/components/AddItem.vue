<script setup>
import { ref } from 'vue'
const newItem = ref('')

const props = defineProps({ list: Object })
// const existingItems = ref(props.list[items])
// console.log(existingItems.value)

const emit = defineEmits(['itemCreated'])

const resetItem = () => {
  newItem.value = ''
}

const addNewItem = () => {
  if (!newItem) return
  fetch('http://localhost:3000/lists/' + props.list.id, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: props.list.title,
      items: [
        ...props.list.items,
        {
          id: props.list.items.length + 1,
          itemName: newItem.value,
          purchased: false
        }
      ],
      updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((updatedList) => {
      emit('itemCreated', updatedList)
      resetItem()
    })
}
</script>

<template>
  <form class="itemForm" @submit.prevent="addNewItem">
    <input v-model="newItem" type="text" placeholder="Add Item" />
    <button type="submit" :disabled="!newItem">Add An Item</button>
  </form>
</template>
