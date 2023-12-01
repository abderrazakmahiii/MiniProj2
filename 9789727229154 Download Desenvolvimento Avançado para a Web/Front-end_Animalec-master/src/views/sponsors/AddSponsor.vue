<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Adicionar Sponsor"/>
  
        <!-- FORM -->
        <b-row>
          <b-col cols="2"></b-col>
          <b-col cols="8">
            <form @submit.prevent="add">
              <div class="form-group">
                <input
                  v-model="name"
                  type="text"
                  class="form-control"
                  id="txtName"
                  placeholder="escreve o nome"
                  required
                />
              </div>
  
              <button type="submit" class="btn btn-outline-success mr-2">
                <i class="fas fa-save"></i> GRAVAR SPONSOR
              </button>
              <router-link
                :to="{ name: 'listSponsors' }"
                tag="button"
                class="btn btn-outline-danger"
              >
                <i class="fas fa-window-close"></i> CANCELAR
              </router-link>
            </form>
          </b-col>
          <b-col cols="2"></b-col>
        </b-row>
      </b-container>
    </section>
  </template>
  
  <script>
  import { ADD_SPONSOR } from "@/store/sponsors/sponsor.constants";
  import HeaderPage from "@/components/HeaderPage.vue";
  import router from "@/router";
  
  import { mapGetters } from "vuex";
  
  export default {
    name: "AddSponsor",
    components: {
      HeaderPage,
    },
    data: () => {
      return {
        name: "",
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getMessage"]),
    },
    methods: {
      add() {
        this.$store
          .dispatch(`sponsor/${ADD_SPONSOR}`, {
            name: this.name,
          })
          .then(
            () => {
              this.$alert(this.getMessage, "Sponsor adicionado!", "success");
              router.push({ name: "listSponsors" });
            },
            (err) => {
              this.$alert(`${err.message}`, "Erro", "error");
            }
          );
      },
    },
  };
  </script>
  