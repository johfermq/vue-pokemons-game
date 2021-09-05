<template>
  <div class="main vh-100">
    <div class="container">
      <PokemonPage />
    </div>
  </div>
</template>

<script setup>
import { defineComponent } from "vue";

/** Components */
import PokemonPage from "@/pages/PokemonPage.vue";

defineComponent({
  PokemonPage,
});
</script>

<style lang="scss" scoped>
.main {
  background-image: url("./assets/background.jpeg");
  background-repeat: no-repeat;
  background-size: cover;
  filter: brightness(0.85);
}
</style>