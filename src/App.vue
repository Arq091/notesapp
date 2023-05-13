<script setup>
import PlusIcon from "./components/icons/PlusIcon.vue"
import TrashIcon from "./components/icons/TrashIcon.vue"
import CloseIcon from "./components/icons/CloseIcon.vue"
import SaveIcon from "./components/icons/SaveIcon.vue"
import LoadIcon from "./components/icons/LoadIcon.vue"

import { ref } from "vue"

const showModal = ref(false)
const noteText = ref("")
const color = ref("")
const errorMsg = ref("")
const notes = ref([])

const addNote = () => {
  if (noteText.value.length < 5) {
    errorMsg.value = "Please enter at least 5 characters"
    return
  } else {
    errorMsg.value = ""
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100000000),
    text: noteText.value,
    date: new Date().toLocaleDateString(),
    color: "hsl(" + color.value + ", 100%, 95%)",
    borderColor: "hsl(" + color.value + ", 100%, 85%)",
  })
  noteText.value = ""
}

const saveNotes = () => {
  localStorage.setItem("myNotes", JSON.stringify(notes.value))
}

const loadNotes = () => {
  if (localStorage.getItem("myNotes") === null) {
    return
  } else {
    notes.value = JSON.parse(localStorage.getItem("myNotes"))
  }
}
</script>

<template>
  <Transition>
    <div
      class="bg-black/50 absolute h-[100vh] w-[100vw] top-0 left-0 flex items-center justify-center"
      v-if="showModal"
    >
      <div
        class="bg-white p-3 rounded flex flex-col gap-2 shadow-lg shadow-black/20 w-full max-w-max m-4"
      >
        <div class="flex">
          <p class="text-xl font-medium">Add a new Note</p>
          <CloseIcon
            class="ml-auto cursor-pointer"
            @click="showModal = false"
          />
        </div>
        <textarea
          v-model="noteText"
          name=""
          id=""
          cols="50"
          rows="8"
          class="resize-none p-4 text-sm bg-gray-100 border border-gray-300 rounded-lg outline-none"
          placeholder="enter some text..."
        ></textarea>
        <p v-if="errorMsg" class="text-red-500 mt-[-5px] text-sm text-bold">
          {{ errorMsg }}
        </p>
        <div class="flex items-center gap-2">
          <p>Color:</p>
          <div
            class="bg-blue-100 rounded border border-blue-300 cursor-pointer w-5 h-5 active:border-black"
            @click="color = '214'"
          ></div>
          <div
            @click="color = '0'"
            class="bg-red-200 rounded border border-red-300 cursor-pointer w-5 h-5 active:border-black"
          ></div>
          <div
            @click="color = '326'"
            class="bg-pink-200 rounded border border-pink-300 cursor-pointer w-5 h-5 active:border-black"
          ></div>
          <div
            @click="color = '32'"
            class="bg-orange-200 rounded border border-orange-300 cursor-pointer w-5 h-5 active:border-black"
          ></div>
          <div
            @click="color = '53'"
            class="bg-yellow-200 rounded border border-yellow-400 cursor-pointer w-5 h-5 active:border-black"
          ></div>
          <div
            @click="color = '141'"
            class="bg-green-200 rounded border border-green-300 cursor-pointer w-5 h-5 active:border-black"
          ></div>
        </div>
        <button class="bg-black text-white rounded-lg py-2" @click="addNote()">
          Add Note
        </button>
      </div>
    </div>
  </Transition>
  <header class="flex justify-between pb-4 border-b-2 border-gray-400 mb-12">
    <h1 class="text-gray-800 text-4xl font-bold">Notes</h1>
    <div class="flex gap-2">
      <button
        class="text-green-600 border-2 border-green-600 px-[0.4rem] rounded-[100%] h-full"
        @click="saveNotes()"
      >
        <LoadIcon />
      </button>
      <button
        class="text-blue-500 border-2 border-blue-500 px-[0.4rem] rounded-[100%] h-full"
        @click="loadNotes()"
      >
        <SaveIcon />
      </button>
      <button
        class="text-red-600 border-2 border-red-600 px-[0.4rem] rounded-[100%] h-full"
        @click="notes = []"
      >
        <TrashIcon />
      </button>
      <button
        class="text-black border-2 border-black px-[0.4rem] rounded-[100%] h-full"
        @click="showModal = true"
      >
        <PlusIcon />
      </button>
    </div>
  </header>
  <main class="flex gap-4 flex-wrap">
    <article
      class="card"
      v-for="note in notes"
      :key="note.id"
      :style="{ backgroundColor: note.color, borderColor: note.borderColor }"
    >
      <p>
        {{ note.text }}
      </p>
      <div class="flex justify-between">
        <p class="font-bold">{{ note.date }}</p>
        <button
          class="text-sm text-white bg-red-700 px-2 rounded"
          @click="
            notes.splice(
              notes.findIndex((item) => item.id === note.id),
              1
            )
          "
        >
          delete
        </button>
      </div>
    </article>
    <div
      class="italic text-center max-w-[20rem] mt-20 text-gray-500 text-sm space-y-4 mx-auto"
      v-if="notes.length == 0"
    >
      <p>
        You haven't added a note yet. Click the "+" icon on the top right to add
        a note.
      </p>
      <p>This is a vue test application made by Arq091 (BustedFridge).</p>
      <p>
        How to use: <br />
        the green icon is for saving changes to the browser, the blue one is for
        loading notes from the browser to the app, the red one is for deleting
        <strong>all</strong> notes (remember, if you want these changes to save,
        press the green icon) , and finally, the black + icon is to add more
        notes (not automatically saved).
      </p>
      <p>enjoy :)</p>
    </div>
  </main>
</template>

<style>
.v-enter-active,
.v-leave-active {
  transition: opacity 100ms ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.card {
  @apply max-w-[20rem] p-4 rounded-lg flex flex-col justify-between min-h-[16rem] border-2 w-full;
}
</style>
