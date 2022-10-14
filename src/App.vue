<template>
  <h1>🎥 IronContacts 🎥</h1>
  <table>
    <thead>
      <tr>
        <th>Picture</th>
        <th v-on:click="sortContacts('n')" class="interactive">Name</th>
        <th v-on:click="sortContacts('p')" class="interactive">Popularity</th>
        <th>Won Oscar</th>
        <th>Won Emmy</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody class="scroll">
      <tr v-for="contact in contacts">
        <td>
          <img :src="contact.pictureUrl" :alt="'Photo of ' + contact.name" />
        </td>
        <td>{{ contact.name }}</td>
        <td>{{ contact.popularity.toFixed(2) }}</td>
        <td v-if="contact.wonOscar">🏆</td>
        <td v-else></td>
        <td v-if="contact.wonEmmy">🌟</td>
        <td v-else></td>
        <td><button @click="deleteContact(contact.id)">Delete</button></td>
      </tr>
    </tbody>
  </table>
  <button @click="addRandomContact">Add Random Contact</button>
</template>

<script setup>
import { ref } from "vue";
import contactsAll from "./data/contacts.json";
const contacts = ref(contactsAll.slice(0, 5));
const addRandomContact = () => {
  contacts.value.push(
    Array.from(contactsAll)
      .sort(() => Math.random() - 0.5)
      .find((c) => !contacts.value.includes(c))
  );
};
const sortContacts = (field = "n") => {
  contacts.value.sort((a, b) => {
    switch (field) {
      case "p":
        if (a.popularity < b.popularity) return 1;
        else if (a.popularity > b.popularity) return -1;
      default:
        if (a.name > b.name) return 1;
        else return -1;
    }
  });
};

const deleteContact = (id) => {
  contacts.value.splice(contacts.value.findIndex((c) => c.id === id), 1);
};
</script>

<style scoped>
h1 {
  text-align: center;
}

button {
  display: block;
  margin: 0 auto;
}

table {
  margin: 2rem auto;
  border-spacing: 0;
}

.scroll {
  overflow: hidden;
  overflow-y: auto;
  max-height: 500px;
}

.interactive {
  text-decoration: underline;
}
.interactive:hover {
  color: blue;
}

tr {
  transition: background-color 300ms;
}

tr:hover:has(td) {
  background-color: aquamarine;
}

th {
  cursor: pointer;
}

th,
td {
  vertical-align: middle;
  text-align: center;
  padding: 0.5rem 0;
  min-width: 120px;
}

img {
  display: block;
  margin: 0 auto;
  max-height: 125px;
}
</style>
