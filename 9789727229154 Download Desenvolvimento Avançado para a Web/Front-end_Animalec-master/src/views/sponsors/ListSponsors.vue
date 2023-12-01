<template>
    <section class="page-section">
      <b-container>
        <HeaderPage title="Sponsors management" />
        <!-- MENU DE TOPO -->
        <b-row class="mb-4">
          <b-col cols="1"></b-col>
          <b-col>
            <router-link
              :to="{ name: 'addSponsor' }"
              tag="button"
              class="btn btn-outline-success mr-2 mt-2"
            >
              <i class="fas fa-plus-square"></i> Add a sponsor
            </router-link>
            <router-link
              :to="{ name: 'admin' }"
              tag="button"
              class="btn btn-outline-info mr-2 mt-2"
            >
              <i class="fas fa-bars"></i> Principal menu
            </router-link>
          </b-col>
          <b-col cols="1"></b-col>
        </b-row>
        <!-- TABLE -->
        <b-row>
          <b-col cols="1"></b-col>
          <b-col>
            <table class="table table-striped">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">ID</th>
                  <th scope="col">NAME</th>
                  <th scope="col">BUDGET</th>
                  <th scope="col">ACTIONS</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="sponsor in sponsors" :key="sponsor._id">
                  <td class="pt-4">{{ sponsor.id }}</td>
                  <td class="pt-4">{{ sponsor.name }}</td>
                  <td class="pt-4">{{ sponsor.budget }}</td>
                  <td>
                    <button
                      @click="viewSponsor(sponsor._id)"
                      type="button"
                      class="btn btn-outline-success mr-2"
                    >
                      <i class="fas fa-search"></i> VIEW
                    </button>
                    <router-link
                      :to="{ name: 'editSponsor', params: { sponsorId: sponsor._id } }"
                      tag="button"
                      class="btn btn-outline-warning mr-2"
                    >
                      <i class="fas fa-edit"></i> EDIT
                    </router-link>
                    <button
                      @click="removeSponsor(sponsor._id)"
                      type="button"
                      class="btn btn-outline-danger"
                    >
                      <i class="fas fa-trash-alt"></i> DELETE
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </b-col>
          <b-col cols="1"></b-col>
        </b-row>
      </b-container>
    </section>
  </template>
  
  <script>
  import { FETCH_SPONSORS, REMOVE_SPONSOR } from "@/store/sponsors/sponsor.constants";
  import { mapGetters } from "vuex";
  import HeaderPage from "@/components/HeaderPage.vue";
  import router from "@/router";
  
  export default {
    name: "ListSponsors",
    components: {
      HeaderPage,
    },
    data: function () {
      return {
        sponsors: [],
      };
    },
    computed: {
      ...mapGetters("sponsor", ["getSponsors", "getMessage"]),
    },
    methods: {
      fetchSponsors() {
        this.$store.dispatch(`sponsor/${FETCH_SPONSORS}`).then(
          () => {
            this.sponsors = this.getSponsors;
          },
          (err) => {
            this.$alert(`${err.message}`, "Error", "error");
          }
        );
      },
      viewSponsor(id) {
        const sponsor = this.sponsors.find((s) => s._id === id);
        this.$fire({
          title: sponsor.name,
          html: `<p><b>Budget:</b> ${sponsor.budget}</p>`,
        });
      },
      removeSponsor(id) {
        this.$confirm(
          "Are you sure you want to delete this sponsor?",
          "Confirmation",
          "warning",
          { confirmButtonText: "OK", cancelButtonText: "Cancel" }
        ).then(
          () => {
            this.$store.dispatch(`sponsor/${REMOVE_SPONSOR}`, id).then(() => {
              this.$alert(this.getMessage, "Sponsor removed!", "success");
              this.fetchSponsors();
            });
          },
          () => {
            this.$alert("Deletion canceled!", "Information", "info");
          }
        );
      },
    },
    created() {
      this.fetchSponsors();
    },
  };
  </script>
  