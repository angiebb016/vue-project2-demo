<script setup>
  import { ref } from 'vue';

  const showModal = ref(false)
  const newNote = ref("");
  const errorMessage = ref("")
  const notes = ref([]);

  function getRandomColor() {
    const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
    return color;
  }

  const addNote = () => {
    if(newNote.value.length < 10) {
      return errorMessage.value = "Note needs to be 10 characters or more.";
    }
    notes.value.push({
      id: Math.floor(Math.random() * 100000),
      text: newNote.value,
      date: new Date(),
      color: getRandomColor()
    });
    showModal.value = false;
    newNote.value = ""
    errorMessage.value = ""
  }

</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        {{ newNote }}
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div 
        v-for="note in notes" 
        :key="note.id"
        class="card" 
        :style="{backgroundColor: note.color}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto
  }

  button{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21,20,20);
    border-radius: 1000px;
    color: white;
    font-size: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 25px;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237,182,44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .main-text {
    line-height: 1.25;
    font-size: 17.5px;
    font-weight: bold;
  }

  .date {
    font-size: 12.5px;
    font-weight: auto;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }

  .modal .close {
    background-color: rgb(213, 11, 11);
    margin-top: 7px;
  }

  .modal p {
    color: rgb(213, 11, 11)
  }
</style>