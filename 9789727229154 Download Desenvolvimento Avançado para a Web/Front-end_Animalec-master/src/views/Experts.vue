<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Experts" />
        <b-row> 
        </b-row>
        <div class="row">
          <Expert v-for="expert in filteredExperts" :key="expert._id" :expert="expert"></Expert>
        </div>
      </b-container>
    </section>
  </template>
  
  <script>
  import Expert from "@/components/Expert.vue";
  import HeaderPage from "@/components/HeaderPage.vue";
  import { FETCH_EXPERTS } from "@/store/experts/expert.constants";
  
  import { mapGetters } from "vuex";
  
  export default {
    name: "Experts",
    components: {
      HeaderPage,
      Expert
    },
    data: function() {
      return {
      };
    },
    computed: {
      ...mapGetters("expert", ["getExperts"]),
      filteredExperts() {
        return this.getExperts;
      }
    },
    created() {
      this.$store
        .dispatch(`expert/${FETCH_EXPERTS}`)
        .then(
          () => {
          },
          err => this.$alert(`${err.message}`, "Error", "error")
        );
    }
  };
  </script>

  