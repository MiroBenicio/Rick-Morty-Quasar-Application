<template>
  <q-layout>
    <q-page-container>
      <q-page class="q-pa-md flex flex-center row items-start q-gutter-md">
        <q-card class="my-card shadow-5">
          <q-card-section>
            <div class="text-center text-bold text-h5">
              {{ personagem.name }}
            </div>
          </q-card-section>
          <q-card-section>
            <img :src="personagem.image" alt="" />
          </q-card-section>
          <q-card-section>
            <div class="">
              <span class="text-bold">Status:</span> {{ personagem.status }}
            </div>
            <div class="">
              <span class="text-bold">Espécie:</span> {{ personagem.species }}
            </div>
            <div class="" v-if="personagem.type != ''">
              <span class="text-bold">Tipo:</span> {{ personagem.type }}
            </div>
            <div class="">
              <span class="text-bold">Gênero:</span> {{ personagem.gender }}
            </div>
            <div class="">
              <span class="text-bold">Planeta de Origem:</span>
              {{ planeta.origem }}
            </div>
            <div class="">
              <span class="text-bold">Localização atual:</span>
              {{ planeta.atual }}
            </div>
          </q-card-section>
        </q-card>
        <q-card class="my-card shadow-5">
          <q-card-section>
            <div class="text-center text-h5">
              Quantidade de episódios: {{ episodios }}
            </div>
          </q-card-section>
          <q-card-section>
            <div class="text-center text-h6">Lista de Episódios</div>
          </q-card-section>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "DetalhesPersonagens",
  data() {
    return {
      personagem: [],
      planeta: {
        origem: "",
        atual: "",
      },
      episodios: 0,
    };
  },
  async created() {
    let resposta = await this.$axios.request({
      url: `https://rickandmortyapi.com/api/character/` + this.$route.params.id,
      method: "get",
    });
    if (resposta.status === 200) {
      this.personagem = resposta.data;
      this.planeta.origem = this.personagem.origin.name;
      this.planeta.atual = this.personagem.location.name;
      this.episodios = this.personagem.episode.length;
    }
  },
});
</script>

<style lang="sass" scoped>
.my-card
    border: 1px solid gray
</style>
