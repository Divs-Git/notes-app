<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          name="note"
          id="note"
          cols="30"
          rows="10"
          placeholder="Enter your note"
          v-model.trim="newNote"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button class="add" @click="handleAddNote">Add Note</button>
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
          :style="{ backgroundColor: note.bgColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date.toLocaleDateString('en-IN') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')

function getRandomColor() {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
}

const handleAddNote = () => {
  if (newNote.value.length >= 10) {
    notes.value.push({
      id: Math.floor(Math.random(100000000)),
      text: newNote.value,
      date: new Date(),
      bgColor: getRandomColor(),
    })
    showModal.value = false
    newNote.value = ''
    errorMessage.value = ''
  } else {
    errorMessage.value = 'Notes need to be 10 characters more'
    return
  }
}
</script>

<style lang="scss">
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
  padding-top: 50px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 60px;
}

h1 {
  font-weight: bold;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: $dark-color;
  border-radius: 100%;
  color: white;
  font-size: 20px;

  &:hover {
    background-color: color.adjust($dark-color, $lightness: 10%);
  }
}

.card {
  width: 225px;
  height: 225px;
  padding: 12px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
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
  justify-content: center;
  align-items: center;
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

.modal {
  button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;

    &.add {
      background-color: $primary-color;

      &:hover {
        background-color: color.adjust($primary-color, $lightness: -10%);
      }
    }

    &.close {
      background-color: $danger-color;

      &:hover {
        background-color: color.adjust($danger-color, $lightness: -10%);
      }
    }
  }
}

.modal p {
  color: color.adjust($danger-color, $lightness: -10%);
}
</style>
