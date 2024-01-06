<script setup>
import { ref } from 'vue'
// const newListAfterDeleting = ref('')
// const props = defineProps({ list: Object })

// const resetList = () => {
//   newListAfterDeleting.value = ''
// }
const emit = defineEmits(['listDeleted'])

const deleteList = () => {
  // props.itemProp.id
  // if (!newListAfterDeleting) return
  // fetch('http://localhost:3000/lists/' + props.list.id, {
  //   method: 'DELETE'
  //   // headers: { 'Content-Type': 'application/json' },
  //   // body: JSON.stringify({
  //   //   title: newListAfterDeleting.value,
  //   //   items: [],
  //   //   updatedAt: new Date()
  //   // })
  // })
  //   .then((res) => res.json())
  //   .then((deletedList) => {
  emit('listDeleted')
  // resetList()
  // })
}
</script>

<template>
  <!-- <form id="newListAfterDeleting" @submit.prevent="deleteList"> -->
  <!-- <h2>{{ newList }}</h2> -->
  <div class="delete">
    <!-- <h3>Are you sure you want to delete {{ props.list.title }}?</h3> -->
    <button @click="deleteList">Remove List</button>
    <!-- <button @click="cancelDelete">Cancel</button> -->
  </div>
  <!-- </form> -->
</template>
