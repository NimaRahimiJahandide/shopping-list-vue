<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const reversedItems = computed(()=> [...items.value].reverse()
)
const editing = ref(false)
const items = ref([
  {
    id: 1,
    label: "10 party hats",
    purchased: true,
    highPriority: false
  },
  {
    id: 2,
    label: "2 board games",
    purchased: true,
    highPriority: false
  },
  {
    id: 3,
    label: "20 cups",
    purchased: false,
    highPriority: true
  }
])
const newItem = ref('')
const newItemPriority = ref(false)
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value, highPriority: newItemPriority.value })
  newItem.value = ''
  newItemPriority.value = ''
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button class="btn" v-if="editing" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button>
  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" type="text" placeholder="Add an item">
    <label>
      <input type="checkbox" v-model="newItemPriority">
      High Priority
    </label>
    <button class="btn btn-primary" :disabled="newItem.length < 5">
      Save Item
    </button>
  </form>
  <p class="counter">
    {{ characterCount }} / 200
  </p>
  <ul>
    <li v-for="(item, index) in reversedItems" :key="item.id"
    class="static-class"
      :class="[
        {strikeout: item.purchased},
        {priority: item.highPriority}
      ]"
      @click="togglePurchased(item)"
      >
      {{ index }} -
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing is here!</p>
</template>