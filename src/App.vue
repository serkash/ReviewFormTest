<script setup>
import ReviewForm from './components/ReviewForm.vue'
</script>

<template>
  <main>
    <ReviewForm />
  </main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

* {
  font-family: 'Roboto', sans-serif;
  box-sizing: border-box;
  margin: 0;
  position: relative;
  font-weight: normal;
}

main {
  background-color: #dfe2e5;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>
