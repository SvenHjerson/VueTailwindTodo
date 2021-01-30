<template>
  <div id="app">
    <div>
      <Todos />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Todos from "@/components/Todos.vue"

export default {
  name: "Home",
  components: {
    Todos
  }
}
</script>

<style lang="css">
::-webkit-input-placeholder {
  font-style: italic;
}
:-moz-placeholder {
  font-style: italic;
}
::-moz-placeholder {
  font-style: italic;
}
:-ms-input-placeholder {
  font-style: italic;
}
</style>
