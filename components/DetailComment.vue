<template>
  <div class="container">
    <NuxtLink to="/">Главная</NuxtLink>
    <div v-if="items">
      <h3>Подробная информация о комментарии</h3>
      <div v-for="(item, index) of items" :key="index">
        <p>{{index + ": " + item}}</p>
      </div>
    </div>
    <div v-else><h3>Подробная информация о комментарии отсутствует</h3></div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Object,
      default: () => {},
    },
  }
}
</script>

<style>
  .container {
    margin: 0 auto;
    position: relative;
    max-width: 100%;
    width: 1440px;
  }
</style>
