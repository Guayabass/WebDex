<template>
  <div class="header">
    <h1 class="title">
      {{ title }}
    </h1>
  </div>
</template>

<script>

export default {
    // eslint-disable-next-line vue/multi-word-component-names, vue/no-reserved-component-names
    name: 'Header',
    data() {
        return {
            title: "Pokemon API"
        }
    }
}
</script>

<style scoped>
.header {
    color: white;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
}

.title {
    color: white;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 15px;
    text-align: center;
}
</style>