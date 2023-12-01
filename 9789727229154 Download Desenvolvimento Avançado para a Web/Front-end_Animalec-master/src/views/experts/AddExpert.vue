<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Add Expert" />
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
                  placeholder="Enter the expert's name"
                  required
                />
              </div>
              <div class="form-group">
                <input
                  v-model="specialty"
                  type="text"
                  class="form-control"
                  id="txtSpecialty"
                  placeholder="Enter the expert's specialty"
                  required
                />
              </div>
  
              <button type="submit" class="btn btn-outline-success mr-2">
                <i class="fas fa-save"></i> SAVE EXPERT
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
  import { ADD_EXPERT } from "@/store/experts/expert.constants";
  import HeaderPage from "@/components/HeaderPage.vue";
  import router from "@/router";
  
  export default {
    name: "AddExpert",
    components: {
      HeaderPage,
    },
    data: () => {
      return {
        name: "",
        specialty: "",
      };
    },
    methods: {
      add() {
        this.$store
          .dispatch(`expert/${ADD_EXPERT}`, {
            name: this.name,
            specialty: this.specialty,
          })
          .then(
            () => {
              this.$alert("Expert added!", "Success", "success");
              router.push({ name: 'listExperts' });
            },
            (err) => {
              this.$alert(`${err.message}`, "Error", "error");
            }
          );
      },
    },
  };
  </script>
  