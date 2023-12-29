<script setup>
import { ref } from 'vue'

const props = defineProps({ item: Object })
</script>

<template>
  <ul>
    <li v-for="prop in props">
      <!-- {{ prop.items.itemName }} -->
      {{ console.log(prop) }}
      <!-- <input v-model="purchased" type="checkbox" /> -->
    </li>
  </ul>
</template>
