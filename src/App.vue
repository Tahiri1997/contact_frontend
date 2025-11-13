<template>
  <div class="container">
    <h1>Formulaire de contact</h1>
    <form @submit.prevent="envoyer">
      <input v-model="nom" placeholder="Nom" required />
      <input v-model="prenom" placeholder="Prénom" required />
      <textarea v-model="commentaire" placeholder="Commentaire" required></textarea>
      <button type="submit">Envoyer</button>
    </form>

    <h2>Contacts enregistrés :</h2>
    <ul>
      <li v-for="c in contacts" :key="c.id">
        {{ c.nom }} {{ c.prenom }} - {{ c.commentaire }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const nom = ref('')
const prenom = ref('')
const commentaire = ref('')
const contacts = ref([])

const API_URL = 'http://localhost:8080/api.php'

const envoyer = async () => {
  await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ nom: nom.value, prenom: prenom.value, commentaire: commentaire.value })
  })
  nom.value = prenom.value = commentaire.value = ''
  chargerContacts()
}

const chargerContacts = async () => {
  const res = await fetch(API_URL)
  contacts.value = await res.json()
}

onMounted(chargerContacts)
</script>


<style scoped>
body {
  background: #f3f4f6;
  font-family: "Segoe UI", Arial, sans-serif;
}

.container {
  max-width: 600px;
  margin: 60px auto;
  background: white;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #007bff;
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

input,
textarea {
  padding: 10px 14px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  transition: border 0.2s;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #007bff;
}

button {
  background: #007bff;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  background: #0056b3;
}

h2 {
  margin-top: 30px;
  color: #333;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #f8f9fa;
  border-left: 4px solid #007bff;
  margin: 8px 0;
  padding: 10px;
  border-radius: 6px;
}
</style>
