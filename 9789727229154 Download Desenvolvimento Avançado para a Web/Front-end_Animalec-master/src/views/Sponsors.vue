<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Sponsors" />
        <b-row>
        </b-row>
        <div class="row">
          <Sponsor v-for="sponsor in filteredSponsors" :key="sponsor._id" :sponsor="sponsor"></Sponsor>
        </div>
      </b-container>
    </section>
  </template>
  
  <script>
  import Sponsor from "@/components/Sponsor.vue";
  import HeaderPage from "@/components/HeaderPage.vue";
  import { FETCH_SPONSORS } from "@/store/sponsors/sponsor.constants";
  
  import { mapGetters } from "vuex";
  
  export default {
    name: "Sponsors",
    components: {
      HeaderPage,
      Sponsor
    },
    data: function() {
      return {
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getSponsors"]),
      ...mapGetters("sponsor", ["getSponsors"]),
      filteredSponsors() {
        return this.getSponsors;
      }
    },
    created() {
      this.$store
        .dispatch(`sponsor/${FETCH_SPONSORS}`)
        .then(
          () => {

          },
          err => this.$alert(`${err.message}`, "Error", "error")
        );
    }
  };
  </script>
  
  <style scoped>

  </style>
  