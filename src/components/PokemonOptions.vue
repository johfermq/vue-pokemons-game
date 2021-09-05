<template>
  <div class="row">
    <div class="col">
      <div class="list-group" role="tablist">
        <button
          v-for="(pokemon, index) in pokemons"
          :key="pokemon.id"
          class="list-group-item list-group-item-action"
          :class="{
            active: pokemon.id === selectedPokemonId,
            'bg-success':
              answered && pokemon.id === selectedPokemonId && success,
            'bg-danger':
              answered && pokemon.id === selectedPokemonId && !success,
          }"
          aria-current="true"
          :disabled="disabledListItem"
          @click.prevent="selectPokemon(pokemon)"
        >
          <h5 class="mb-1">
            <span
              class="small text-primary"
              :class="{
                'text-white': answered && pokemon.id === selectedPokemonId,
              }"
              >{{ index + 1 }}.</span
            >
            {{ pokemon.name }}
          </h5>
        </button>
      </div>
    </div>
  </div>
  <div class="row my-4" v-if="answer">
    <div class="col">
      <div
        class="alert alert-light h5 border border-primary"
        role="alert"
        v-html="answer"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";

const props = defineProps({
  pokemons: {
    type: Array,
    required: true,
    default: [],
  },
  answer: {
    type: String,
    required: false,
    default: "",
  },
  success: {
    type: Boolean,
    required: false,
    default: false,
  },
});

const emit = defineEmits(["selection-pokemon"]);

const selectedPokemonId = ref(null);

const selectPokemon = (pokemon) => {
  if (!disabledListItem.value) {
    selectedPokemonId.value = pokemon.id;
    emit("selection-pokemon", pokemon.id);
  }
};

const answered = computed(() => props.answer !== "");

const disabledListItem = computed(
  () => answered.value && props.selectedPokemonId !== null
);
</script>