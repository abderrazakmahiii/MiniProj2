<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Edit Expert" />
        <!-- FORM -->
        <b-row>
          <b-col cols="2"></b-col>
          <b-col cols="8">
            <form @submit.prevent="update">
              <div class="form-group">
                <input
                  v-model="expert.name"
                  type="text"
                  class="form-control"
                  id="txtName"
                  placeholder="Enter the expert's name"
                  required
                />
              </div>
              <div class="form-group">
                <input
                  v-model="expert.specialty"
                  type="text"
                  class="form-control"
                  id="txtSpecialty"
                  placeholder="Enter the expert's specialty"
                  required
                />
              </div>
  
              <button type="submit" class="btn btn-outline-success mr-2">
                <i class="fas fa-save"></i> UPDATE EXPERT
              </button>
              <router-link
                :to="{ name: 'listExperts' }"
                tag="button"
                class="btn btn-outline-danger"
              >
                <i class="fas fa-window-close"></i> CANCEL
              </router-link>
            </form>
          </b-col>
          <b-col cols="2"></b-col>
        </b-row>
      </b-container>
    </section>
  </template>
  
  <script>
  import { EDIT_EXPERT } from "@/store/experts/expert.constants";
  import router from "@/router";
  import HeaderPage from "@/components/HeaderPage.vue";
  
  export default {
    name: "EditExpert",
    components: {
      HeaderPage,
    },
    data: () => {
      return {
        expert: {},
      };
    },
    methods: {
      update() {
        this.$store
          .dispatch(`expert/${EDIT_EXPERT}`, this.$data.expert)
          .then(
            () => {
              this.$alert("Expert updated!", "Success", "success");
              router.push({ name: 'listExperts' });
            },
            (err) => {
              this.$alert(`${err.message}`, "Error", "error");
            }
          );
      },
    },
    created() {
      this.expert = this.getExpertById(this.$route.params.expertId);
    },
    computed: {
      ...mapGetters("expert", ["getExpertById"]),
    },
  };
  </script>
  