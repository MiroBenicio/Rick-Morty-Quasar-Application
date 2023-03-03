<template>
  <q-page class="q-pa-md">
    <q-table
      :rows="personagens"
      :columns="personagensColumns"
      align="left"
      row-key="id"
      :pagination="pagination"
      :loading="loading"
      @update:pagination="(v) => buscar()"
    >
      <template v-slot:top>
        <div class="col-6 text-h5 text-primary">Personagens</div>
        <div class="col-6 q-px-xs">
          <q-input
            placeholder="Pesquise pelo nome do personagem"
            v-model="filtro"
            @update:model-value="buscar()"
            :debounce="400"
          >
            <template v-slot:append>
              <q-icon name="search" color="primary" />
            </template>
          </q-input>
        </div>
      </template>
      <template v-slot:header="props">
        <q-tr :props="props">
          <q-th v-for="col in props.cols" :key="col.name" :props="props">
            <div>{{ col.label }}</div>
          </q-th>
        </q-tr>
      </template>
      <template v-slot:body="props">
        <q-tr
          :props="props"
          clickable
          @click="detalhesPersonagem(props.row.id)"
          class="cursor-pointer"
        >
          <td>
            <q-img :src="props.row.image"></q-img>
          </td>
          <q-td>
            <div class="text-center">{{ props.row.name }}</div>
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "ListaPersonagens",
  data() {
    return {
      personagens: [],
      personagensColumns: [
        { name: "imagem", label: "", field: "image", align: "left" },
        { name: "name", label: "Nome", field: "name", align: "center" },
      ],
      pagination: {
        sortBy: "desc",
        descending: false,
        page: 1,
        rowsPerPage: 10,
        rowsNumber: 0,
      },
      loading: false,
      filtro: "",
    };
  },
  methods: {
    async buscar(props) {
      this.loading = true;
      if (props) {
        this.pagination.page = props.pagination.page;
        this.pagination.rowsPerPage = props.pagination.rowsPerPage;
        this.pagination.sortBy = props.pagination.sortBy;
        this.pagination.descending = props.pagination.descending;
      }
      let data = {
        name: this.filtro,
        ...this.pagination,
      };
      let resposta = await this.$axios.request({
        url: `https://rickandmortyapi.com/api/character/`,
        method: "get",
        params: data,
      });
      if (resposta.status === 200) {
        this.pagination.rowsNumber = parseInt(resposta.data.info.pages);
        this.personagens = resposta.data.results;
      }
      this.loading = false;
    },
    detalhesPersonagem(id) {
      console.log(id);
      this.$router.push("/DetalhesPersonagem/" + id);
    },
  },
});
</script>
