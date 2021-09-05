<template>
  <img
    :src="img"
    class="img-fluid"
    :class="customClass"
    alt="pokemon"
    v-if="img"
  />
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  pokemonId: {
    type: Number,
    required: true,
    default: 0,
  },
  showPokemon: {
    type: Boolean,
    required: true,
    default: false,
  },
  shadowClass: {
    type: String,
    required: true,
    default: "default",
  },
});

const url = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/`;

const img = computed(() => {
  if (props.pokemonId === 0) {
    return;
  }
  return `${url}${props.pokemonId}.svg`;
});

const customClass = computed(() => {
  return {
    "shadow-default": props.shadowClass === "default",
    "shadow-success": props.shadowClass === "success",
    "shadow-error": props.shadowClass === "error",
    "hidden-pokemon": !props.showPokemon,
  };
});
</script>

<style lang="scss" scoped>
.img-fluid {
  height: 13rem;
  user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
}

.hidden-pokemon {
  filter: brightness(0) !important;
  -webkit-filter: brightness(0) !important;
}

.shadow-default {
  filter: drop-shadow(0 0 1rem #000);
  -webkit-filter: drop-shadow(0 0 1rem #000);
}

.shadow-success {
  filter: drop-shadow(0 0 1rem #198754);
  -webkit-filter: drop-shadow(0 0 1rem #198754);
}

.shadow-error {
  filter: drop-shadow(0 0 1rem #dc3545);
  -webkit-filter: drop-shadow(0 0 1rem #dc3545);
}
</style>