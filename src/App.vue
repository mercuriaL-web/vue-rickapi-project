<script setup lang="ts">
import Card from "./components/Card.vue";
import { onMounted, ref } from "vue";
import { debounce } from "throttle-debounce";


const cardsData = ref<any[]>([]);
const dataUrl = ref<string>("https://rickandmortyapi.com/api/character");

async function getData() {
  try {
    const response = await fetch(dataUrl.value);
    const responseData = await response.json();
    cardsData.value.push(...responseData.results);
    dataUrl.value = responseData.info.next;
  } catch (error) {
    console.error("Error: ", error);
  }
}

const cardsContainer = ref<HTMLElement | null>(null);

async function handleScroll() {
  if (!cardsContainer.value) return;
  if (
    cardsContainer.value.scrollTop + cardsContainer.value.clientHeight >=
    cardsContainer.value.scrollHeight
  ) {
    await getData();
  }
}

async function handleSearch(query: HTMLInputElement) {
  try {
    if (!query) return;
    const trimmedQuery = query.value.trim();
    

    const response = await fetch(
      `https://rickandmortyapi.com/api/character/?name=${trimmedQuery}`
    );
    const responseData = await response.json();
    cardsData.value = responseData.results;
  } catch (error) {
    console.error("Error: ", error);
  }
}

const handleSearchWithDebounce = debounce(500, handleSearch);

onMounted(async () => {
  await getData();
});
</script>

<template>
  <main>
    <input
      @input="handleSearchWithDebounce($event.target)"
      type="search"
      placeholder="Search..."
    />
    <div ref="cardsContainer" class="cards" @scroll="handleScroll">
      <Card
        v-for="({ image, name, location }, index) in cardsData"
        :key="index"
        :cardImage="image"
        :cardName="name"
        :cardLocation="location.name"
      />
    </div>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: var(--backgroundColor);
  color: var(--primaryColor);
}

main {
  padding: 10px;
}

input {
  border-radius: 5px;
  background: #3c3c3c;
  color: white;
  border: 3px solid var(--secondaryColor);
  padding: 10px;
  font-weight: bold;
  width: 100%;
}

input[type="search"]:focus {
  outline: 0;
  border: 3px var(--primaryColor) solid;
}

.cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  overflow-y: auto;
  height: 100vh;
}

@media screen and (min-width: 1024px) {
  .cards {
    grid-template-columns: repeat(5, 1fr);
  }
}
</style>
