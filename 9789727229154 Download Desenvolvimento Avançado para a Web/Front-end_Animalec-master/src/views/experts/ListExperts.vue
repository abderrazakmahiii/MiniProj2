<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Experts management" />
      <!-- MENU DE TOPO -->
      <b-row class="mb-4">
        <b-col cols="1"></b-col>
        <b-col>
          <router-link
            :to="{ name: 'addExpert' }"
            tag="button"
            class="btn btn-outline-success mr-2 mt-2"
          >
            <i class="fas fa-plus-square"></i> Add an expert
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
                <th scope="col">SPECIALTY</th>
                <th scope="col">ACTIONS</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="expert in experts" :key="expert._id">
                <td class="pt-4">{{ expert.id }}</td>
                <td class="pt-4">{{ expert.name }}</td>
                <td class="pt-4">{{ expert.specialty }}</td>
                <td>
                  <button
                    @click="viewExpert(expert._id)"
                    type="button"
                    class="btn btn-outline-success mr-2"
                  >
                    <i class="fas fa-search"></i> VIEW
                  </button>
                  <router-link
                    :to="{ name: 'editExpert', params: { expertId: expert._id } }"
                    tag="button"
                    class="btn btn-outline-warning mr-2"
                  >
                    <i class="fas fa-edit"></i> EDIT
                  </router-link>
                  <button
                    @click="removeExpert(expert._id)"
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
import { FETCH_EXPERTS, REMOVE_EXPERT } from "@/store/experts/expert.constants";
import { mapGetters } from "vuex";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "@/router";

export default {
  name: "ListExperts",
  components: {
    HeaderPage,
  },
  data: function () {
    return {
      experts: [],
    };
  },
  computed: {
    ...mapGetters("expert", ["getExperts", "getMessage"]),
  },
  methods: {
    fetchExperts() {
      this.$store.dispatch(`expert/${FETCH_EXPERTS}`).then(
        () => {
          this.experts = this.getExperts;
        },
        (err) => {
          this.$alert(`${err.message}`, "Error", "error");
        }
      );
    },
    viewExpert(id) {
      const expert = this.experts.find((e) => e._id === id);
      this.$fire({
        title: expert.name,
        html: `<p><b>Specialty:</b> ${expert.specialty}</p>`,
      });
    },
    removeExpert(id) {
      this.$confirm(
        "Are you sure you want to delete this expert?",
        "Confirmation",
        "warning",
        { confirmButtonText: "OK", cancelButtonText: "Cancel" }
      ).then(
        () => {
          this.$store.dispatch(`expert/${REMOVE_EXPERT}`, id).then(() => {
            this.$alert(this.getMessage, "Expert removed!", "success");
            this.fetchExperts();
          });
        },
        () => {
          this.$alert("Deletion canceled!", "Information", "info");
        }
      );
    },
  },
  created() {
    this.fetchExperts();
  },
};
</script>
