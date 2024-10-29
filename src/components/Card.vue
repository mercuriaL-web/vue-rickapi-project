<script setup lang="ts">
 defineProps({
    cardImage: String,
    cardName: String,
    cardLocation: String
  })
</script>

<template>
    <div class="card">
        <img :src=cardImage>
        <h2>{{ cardName }}</h2>
        <p>Location: {{ cardLocation }}</p>
    </div>
</template>

<style scoped>
.card {
    border: 2px solid #3c3c3c;
    border-radius: 15px;
    margin: 20px;
    padding: 10px;
    transition: .4s;
    cursor: pointer;
}

.card:hover {
    border-color: var(--primaryColor);
    transform: translateY(-10px);
}

img {
    width: 100%;
    margin-bottom: 10px;
}
</style>