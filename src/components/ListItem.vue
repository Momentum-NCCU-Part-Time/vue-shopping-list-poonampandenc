<script setup>
import { ref } from 'vue'
// import NewListForm from './NewListForm.vue'

const props = defineProps({ item: Object })
</script>

<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">
        {{ list.items.item - name }}
        {{ list.items.purchased }}
      </li>
    </ul>
  </div>
</template>
