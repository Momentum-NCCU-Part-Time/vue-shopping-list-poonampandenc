<script setup>
import { ref } from 'vue'
// import NewListForm from './NewListForm.vue'

const props = defineProps({
  id: number,
  title: String,
  content: String
})
</script>

<template>
  <div class="ListItem">
    <h2>{{ props.title }}</h2>
    <ul>
      <li>
        {{ list.items.item - name }}
        <!-- {{ list.items.purchased }} -->
      </li>
    </ul>
  </div>
</template>
