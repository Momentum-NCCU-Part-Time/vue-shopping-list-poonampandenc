<script setup>
import { ref } from 'vue'

const props = defineProps({ list: Object })
const emit = defineEmits(['listDeleted', 'deleteCanceled', 'deleteConfirmed'])
const deleting = ref(false)

const deleteList = () => {
  fetch('http://localhost:3000/lists/' + props.list.id, {
    method: 'DELETE'
  })
    .then((res) => res.json())
    .then((deletedList) => {
      emit('listDeleted', deletedList)
      deleteConfirmed()
    })
}

const confirmDelete = () => {
  emit('deleteConfirmed', confirmDelete)
}

const cancelDelete = () => {
  emit('deleteCanceled', cancelDelete)
}
</script>

<template>
  <div class="deleting">
    <div class="window">
      <h3>Are you sure you want to delete {{ props.list.title }}?</h3>
      <button @click="deleteList">Delete</button>
      <button @click="cancelDelete">Cancel</button>
    </div>
  </div>
</template>
