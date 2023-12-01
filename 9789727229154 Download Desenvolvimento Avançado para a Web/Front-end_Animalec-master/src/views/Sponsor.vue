<template>
    <section class="page-section">
      <b-container>
        <HeaderPage :title="'Sponsor: ' + sponsor.name" />
        <b-row>
          <b-col cols="1"></b-col>
          <b-col cols="10">
            <b-card>
              <b-card-title>{{ sponsor.name }}</b-card-title>
              <b-card-text>
                <p><strong>Budget:</strong> {{ sponsor.budget }}</p>
              </b-card-text>
            </b-card>
          </b-col>
          <b-col cols="1"></b-col>
        </b-row>
      </b-container>
    </section>
  </template>
  
  <script>
  import HeaderPage from "@/components/HeaderPage.vue";
  import { mapGetters } from "vuex";
  
  export default {
    name: "Sponsor",
    components: {
      HeaderPage,
    },
    data: function () {
      return {
        sponsor: null,
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getSponsorById"]),
    },
    created() {
      this.sponsor = this.getSponsorById(this.$route.params.sponsorId);
    },
  };
  </script>
  
  <style scoped>
  b-card {
    margin-top: 20px;
  }
  </style>
  