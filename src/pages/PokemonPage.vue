<template>
  <h1 class="display-5 py-4">¿Quién es ese pokémon?</h1>
  <div class="vh-75">
    <div class="row align-content-center">
      <div class="col-md-6 my-md-6 text-center">
        <div class="display-6" v-if="loading">
          <div
            class="spinner-grow text-primary"
            style="width: 3rem; height: 3rem"
            role="status"
          >
            <span class="visually-hidden"></span>
          </div>
          <p class="align-middle">Cargando...</p>
        </div>

        <PokemonPicture
          :pokemon-id="pokemon.id"
          :show-pokemon="showPokemon"
          :shadow-class="shadowClass"
          v-else
        />

        <div class="py-sm-5 py-md-4" v-if="showAnswer">
          <button
            type="button"
            class="btn btn-outline-primary"
            @click.prevent="newGame"
          >
            Nueva Jugada
          </button>
        </div>
      </div>
      <div class="col-md-6 my-md-6">
        <div v-if="pokemon">
          <PokemonOptions
            :pokemons="pokemons"
            :answer="message"
            :success="success"
            @selection-pokemon="checkAnswer"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineComponent, ref, onMounted, computed } from "vue";
import swal from "sweetalert2";

/** Components */
import PokemonOptions from "@/components/PokemonOptions.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";

/** Services */
import getPokemonOptions from "@/helpers/getPokemonOptions";

defineComponent({ PokemonOptions, PokemonPicture });

const pokemons = ref([]);
const pokemon = ref({});
const showPokemon = ref(false);
const showAnswer = ref(false);
const loading = ref(false);
const success = ref(false);
const message = ref("");

const fetchPokemons = async () => {
  loading.value = true;
  pokemons.value = await getPokemonOptions();
  loading.value = false;

  const rndInt = Math.floor(Math.random() * 4);
  pokemon.value = pokemons.value[rndInt];
};

const checkAnswer = (selectedId) => {
  showPokemon.value = true;
  showAnswer.value = true;
  let title = "¡Perdiste!";
  let html = `Respuesta correcta: <strong>${pokemon.value.name}</strong>`;
  let icon = "error";

  if (selectedId === pokemon.value.id) {
    success.value = true;
    title = "Ganaste!";
    html = `Respuesta correcta: <strong>${pokemon.value.name}</strong>`;
    icon = "success";
  }

  message.value = html;

  swal.fire({
    title,
    html,
    icon,
    allowOutsideClick: false,
    allowEscapeKey: false,
    confirmButtonColor: "#0d6efd",
  });
};

const shadowClass = computed(() => {
  if (!showPokemon.value) {
    return "default";
  }

  if (success.value) {
    return "sucsess";
  }

  return "error";
});

const newGame = () => {
  showPokemon.value = false;
  showAnswer.value = false;
  pokemons.value = [];
  pokemon.value = {};
  success.value = false;
  message.value = "";
  fetchPokemons();
};

onMounted(() => fetchPokemons());
</script>

<style lang="scss" scoped>
.display-5 {
  font-weight: bold;
}

.my-6 {
  margin-top: 6rem !important;
  margin-bottom: 6rem !important;
}

.my-md-6 {
  margin-top: 6rem !important;
  margin-bottom: 6rem !important;
}
</style>
