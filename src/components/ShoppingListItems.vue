<script setup>
import { ref } from 'vue'
import AddItem from './AddItem.vue'
import Delete from './Delete.vue'

const props = defineProps({ list: Object })
const emit = defineEmits(['itemPurchased'])
const editing = ref(false)
const togglePurchased = (item) => {
  console.log(props.list)
  item.purchased = !item.purchased
  console.log(props.list)
  savePurchasedStatus(props.list)
}
const doEdit = () => {
  // editing.value = e
  editing.value = !editing.value
}

const addNewItem = (newItem) => {
  // props.itemProp.id
  if (!newItem) return
  fetch('http://localhost:3000/lists/' + props.list.id, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: props.list.title,
      items: [...props.list.items, newItem],
      updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((updatedList) => {
      emit('listUpdated', updatedList)
      // resetList()
    })
}
const savePurchasedStatus = (list) => {
  fetch('http://localhost:3000/lists/' + list.id, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: list.title,
      items: [...list.items],
      updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((r) => {})
}

const deleteList = () => {
  // props.itemProp.id
  // if (!newListAfterDeleting) return
  fetch('http://localhost:3000/lists/' + props.list.id, {
    method: 'DELETE'
    // headers: { 'Content-Type': 'application/json' },
    // body: JSON.stringify({
    //   title: [props.list.title],
    //   items: [props.list.items],
    //   updatedAt: new Date()
    // })
  })
    .then((res) => res.json())
    .then((r) => {
      emit('listDeleted', props.list.id)
      // resetList()
    })
}
</script>

<template>
  <div>
    <button>
      <h3 @click="doEdit">
        {{ props.list.title }}: {{ props.list.items.filter((e) => !e.purchased).length }} /
        {{ props.list.items.length }}
      </h3>
    </button>
    <!-- <button v-if="editing" @click="doEdit(false)">Close List</button>
    <button v-else @click="doEdit(true)">Show List</button> -->

    <ul v-if="editing">
      <li
        class="listItem"
        v-for="currentItem in props.list.items"
        @click="togglePurchased(currentItem)"
        :key="currentItem.id"
        :class="{ strikeout: currentItem.purchased }"
      >
        {{ currentItem.itemName }}
        <input v-model="currentItem.purchased" type="checkbox" />
        <!-- {{ list.items.purchased }} -->
      </li>
    </ul>
  </div>
  <div v-if="editing">
    <AddItem :nextId="props.list.items.length + 1" @itemCreated="addNewItem" />
    <!-- <div v-if="editing">
      <AddItem @itemCreated="addNewItem" />
    </div> -->
    <!-- <button v-if="editing" @click="savePurchasedStatus(items)">Save Updated List</button> -->
    <!-- <button v-if="deleting" @click="deleteList">Delete</button> -->
    <Delete @listDeleted="deleteList" />
  </div>
</template>

<style>
.listItem {
  list-style: none;
}

.strikeout {
  text-decoration: line-through;
  color: blueviolet;
}

.strikeout:hover {
  color: brown;
}
</style>
