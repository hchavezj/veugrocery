<script setup>
  import {ref} from 'vue'
  import { nanoid } from 'nanoid'
  import {useStorage} from '@vueuse/core'
  import confetti from 'canvas-confetti'

  const newGrocery = ref('')
  const groceries = useStorage('groceries', [])

  const addGrocery = ()=> {
    if (newGrocery.value) {
      groceries.value.push({id: nanoid(), name: newGrocery.value})
      newGrocery.value = '';
    }
  }
  const deleteGrocery = id => {
    const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
    groceries.value.splice(removeIndex, 1)
    confetti({ particleCount: 300, spread: 1000, origin: { y: 1 } })
  }
</script>

<template>
  <body>
    <main>

      <h1 class="title">📝 Vue Grocery List 📝</h1>

      <form class="newGroceryForm" @submit.prevent="addGrocery">
        <input
          id="newGrocery"
          autocomplete="off"
          type="text"
          placeholder="Add an item to your list"
          v-model="newGrocery"
        />
        <button type="submit">Add</button>
      </form>
      <h3>Pending Items: {{groceries.length}}</h3>
      <ul>

        <li v-for="grocery in groceries" @click="deleteGrocery(grocery.id)">
          {{ grocery.name }}
        </li>
      </ul>
    </main>
  </body>
</template>

<style lang="postcss" scoped>
  body {
    height: 100vh;
    @apply bg-Base text-Text;
    main {
      @apply flex flex-col pt-8 justify-center items-center;
      .title {
        @apply m-2 text-6xl font-light text-Text;
      }
      form {
        @apply flex m-8 focus-within:ring-8 focus-within:ring-Green focus-within:rounded-lg;
        input {
          @apply bg-white p-2 w-80 text-Subtext1 rounded-l-md;
        }
        button {
          @apply bg-Green text-Crust text-2xl font-semibold p-2 rounded-r-md;
          &:hover {
            @apply bg-purple-500;
        }
      }
    }
    ul {
      @apply flex flex-col items-center justify-center rounded-lg bg-Lavender;
      li {
        @apply bg-white text-Base m-2 p-2 w-96 text-center rounded-md;
        &:hover {
          @apply bg-purple-500 font-bold cursor-pointer;
        }
      }
    }
  }
}
</style>
