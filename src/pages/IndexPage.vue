<template>
  <q-page>
    <lista-personagens />
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import ListaPersonagens from "components/ListaPersonagens.vue";

export default defineComponent({
  name: "IndexPage",
  components: { ListaPersonagens },
});
</script>
