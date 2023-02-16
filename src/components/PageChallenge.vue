<script setup lang="ts">
// defineProps<{
//   msg: string
// }>()
</script>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      firstname: "Jean",
      age: 30
    }
  },
  mounted(){
    setInterval(age += 3, 3000)
  }
})
</script>

<template>
  <div id="presentation">
    <p>Bonjour je m'appelle {{ firstname }}</p>
  </div>
  <div id="age">
    <p>En 2022, {{ firstname }} aura 23 ans</p>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
