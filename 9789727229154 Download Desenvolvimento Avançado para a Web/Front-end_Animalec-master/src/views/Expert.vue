<template>
    <section class="page-section">
      <b-container>
        <HeaderPage :title="'Expert: ' + expert.name" />
        <b-row>
          <b-col cols="1"></b-col>
          <b-col cols="10">
            <b-card>
              <b-card-title>{{ expert.name }}</b-card-title>
              <b-card-text>
                <p><strong>Expert:</strong> {{ expert.speciality }}</p>
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
    name: "Expert",
    components: {
      HeaderPage,
    },
    data: function () {
      return {
        expert: null,
      };
    },
    computed: {
      ...mapGetters("expert", ["getExpertById"]),
    },
    created() {
      this.expert = this.getExpertById(this.$route.params.expertId);
    },
  };
  </script>
  
  <style scoped>
  b-card {
    margin-top: 20px;
  }
  </style>
  