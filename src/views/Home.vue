<template>
  <div class="home d-flex">
    <chapter1-4 class="flex-grow-1" msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import chapter1_4 from "@/components/chapter1.4.vue";

export default {
  name: "home",
  components: {
    "chapter1-4": chapter1_4
  }
};
</script>
