<template>
  <div class="publicationDiv">
        <b-table
          class="publicationTable"
          :fields="fields"
          :items="allPublications"
          :head-variant="light"
          :bordered="false"
          :responsive="md"
          id="main_table"
          :per-page="perPage"
          :current-page="currentPage"
          hover
        >
          <template #cell(submissionDate)="data">
            <p>
              {{ data.item.submissionDate.slice(0, 10) }}
            </p>
          </template>
          <template #cell(status)="data">
            <p v-if="data.item.status == 0">Under Review</p>
            <p v-else>Published</p>
          </template>
        </b-table>
      <div id="paginate">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="main_table"
          align="center"
          pills
        ></b-pagination>
      </div>
    <br />
  </div>
</template>


<script>
//import ActionButton from "./../Shared/ActionButton";

export default {
  name: "PublicationsTable",
  data() {
    return {
      showModal: false,
      perPage: 5,
      currentPage: 1,
      sortBy: "name",
      publications: [],
      allPublications: [],
      organization: {},
      organizationIdentifier: null,
      response: null,
      InProgress: true,
      networkConnected: null,
      fields: ["title", "submissionDate", "status"],
      selectedTab: "Active",
    };
  },
  async mounted() {
    //this.$store.commit("global/toggleLoading", "on");
    this.allPublications = this.$store.getters["publication/publications"];
    this.filterPublications();
    if (this.allPublications.length == 0 || this.allPublications == undefined) {
      this.refresh();
    }
    this.$store.commit("global/toggleLoading", "off");
  },
  components: {
    // ActionButton,
  },
  methods: {
    tabSelect(tab) {
      console.log(tab);
      this.selectedTab = tab;
      this.filterPublications();
    },
    filterPublications() {
      if (this.selectedTab === "Articles") {
        this.publications = this.allPublications.filter(
          (certificate) => certificate.status == 0
        );
      } else if (this.selectedTab === "Active") {
        this.publications = this.allPublications.filter(
          (certificate) => certificate.status == 0
        );
      } else {
        this.publications = this.allPublications.filter(
          (certificate) => certificate.status == 2
        );
      }
    },
    add() {
      this.$router.push({ name: "UploadPublication" });
    },
    view(item) {
      console.log(item);
      this.$store.dispatch("certificate/changeCertificate", item);
      setTimeout(() => {}, 500);
    },
    view2(record, index) {
      this.$store.dispatch(
        "certificate/changeCertificate",
        this.publications[index]
      );
      this.$router.push({ name: "CertificateProfile" });
    },
    delet() {
      this.$store.dispatch("certificate/deleteCertificate");
      setTimeout(() => {
        this.$alert(this.$store.responseMessage);
      }, 3000);
      this.$store.responseMessage = "_blank_";
    },
    editFromProfile() {
      this.$store.dispatch("certificate/changeMode", "edit");
      this.$router.push({ name: "formPage1" });
    },
    async refresh() {
      this.$store.commit("global/toggleLoading", "on");
      let response = await this.$store.dispatch(
        "publication/fetchPublications"
      );
      if (response.status) {
        this.allPublications = response.publications;
      }
      this.filterPublications();
      this.$store.commit("global/toggleLoading", "off");
    },
  },
  computed: {
    rows() {
      return this.publications.length;
    },
  },
};
</script>

<style>
.publicationDiv {
  font-family: "Montserrat";
  text-align: left;
}
table {
  margin-top: 0px;
}

.certificateName {
  cursor: pointer;
}


.publicationTable{
  margin: -17px 0 0 0;
}

#customButton {
  font-family: "Montserrat";
  background: white;
  padding: 5px;
  color: #1d2029;
  border-radius: 5px;
  border: 2px solid #989898;
  font-weight: 600;
  margin: 1px;
}
.page-item.active .page-link {
  z-index: 3;
  color: #fff;
  background-color: #fe6663;
  border-color: #fe6663;
}
.nav-pills .nav-link.active,
.nav-pills .show > .nav-link {
  color: #fff;

  background-color: #f6a8a8;
  width: 100%;
}
.nav-pills {
  padding: 0;
  background-color: #f8f8f8;
}

a {
  color: #1d2029;
  font-weight: 500;
  text-decoration: none;
  background-color: transparent;
}
a:hover {
  color: #1d2029;
  text-decoration: none;
  background-color: transparent;
}
</style>