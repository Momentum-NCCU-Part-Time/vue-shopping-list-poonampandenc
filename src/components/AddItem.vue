<script setup>
import { ref } from 'vue'
const newItemName = ref('')

const props = defineProps({ nextId: Number })
// const existingItems = ref(props.list[items])
// console.log(existingItems.value)

const emit = defineEmits(['itemCreated'])

const resetItem = () => {
  newItemName.value = ''
}

const createNewItem = () => {
  if (!newItemName) return
  //   fetch('http://localhost:3000/lists/' + props.list.id, {
  //     method: 'PATCH',
  //     headers: { 'Content-Type': 'application/json' },
  //     body: JSON.stringify({
  //       title: props.list.title,
  //       items: [
  //         ...props.list.items,
  //         {
  //           id: props.list.items.length + 1,
  //           itemName: newItem.value,
  //           purchased: false
  //         }
  //       ],
  //       updatedAt: new Date()
  //     })
  //   })
  //     .then((res) => res.json())
  //     .then((updatedList) => {
  emit('itemCreated', {
    id: props.nextId,
    itemName: newItemName.value,
    purchased: false
  })
  resetItem()
  // })
}
</script>

<template>
  <form class="itemForm" @submit.prevent="createNewItem">
    <input v-model="newItemName" type="text" placeholder="Add Item" />
    <button type="submit" :disabled="!newItemName">Add An Item</button>
  </form>
</template>
