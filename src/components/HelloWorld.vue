<template>
  <div>
    <h2>Characters:</h2>
    characters: {{ characters }}
    <article v-for="(characters, idx) in characters" :key="idx">
      <img :src="characters.image">
      <h1>{{ characters.name }}</h1>
      <button @click="deleteCharacters(characters.id)">
        Delete
      </button>
    </article>
    <form @submit="addCharacters(name, image)">
      <input v-model="name" placeholder="Characters Name">
      <input v-model="image" placeholder="Characters Image URL">
      <button type="submit">Add New Characters</button>
    </form>
  </div>
</template>

<script>
import { db } from '../db'
export default {
  name: 'HelloWorld',
  data () {
    return {
      characters: [],
      name: '',
      image: ''
    }
  },
  firestore () {
    return {
      characters: db.collection('characters').orderBy('name')
    }
  },
  methods: {
    addCharacters (name, image) {
      const createdAt = new Date()
      db.collection('characters').add({ name, image, createdAt })
    },
    deleteCharacters (id) {
      db.collection('characters').doc(id).delete()
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
