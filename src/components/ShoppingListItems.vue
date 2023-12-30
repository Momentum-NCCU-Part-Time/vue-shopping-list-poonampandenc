<script setup>
import { ref } from 'vue'
import AddItem from './AddItem.vue'

const props = defineProps({ list: Object })
const emit = defineEmits(['itemPurchased'])
const editing = ref(false)
const togglePurchased = (item) => {
  console.log(props.list)
  item.purchased = !item.purchased
  console.log(props.list)
  savePurchasedStatus(props.list)
}
const doEdit = (e) => {
  editing.value = e
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
</script>

<template>
  <div class="listItem">
    <h2>{{ props.list.title }}: {{ props.list.items.length }}</h2>
    <button v-if="editing" @click="doEdit(false)">Close List</button>
    <button v-else @click="doEdit(true)">Show List</button>

    <ul v-if="editing">
      <li
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
  </div>
</template>

<style>
.strikeout {
  text-decoration: line-through;
  color: blueviolet;
}

.strikeout:hover {
  color: brown;
}
</style>
