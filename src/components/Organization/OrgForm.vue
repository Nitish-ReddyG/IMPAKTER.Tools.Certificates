<template>
  <div class="editMain">
    <h1 class="sectionTitle">Edit Profile</h1>
    <hr />
    <b-row class="main_row">
      <b-col cols="7">
        <b-form @submit="onSubmit" @reset="onReset">
          <b-form-group
            class="title"
            label-cols="1"
            label-cols-lg="6"
            label="Name of the Organization"
            label-for="name"
            label-align-sm="left"
          >
            <b-form-input
              id="name"
              v-model="form.organization.name"
              placeholder="Name"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Phone Number"
            label-for="phone"
            label-align-sm="left"
          >
            <b-form-input
              id="phone"
              v-model="form.orgCommunication.phone"
              placeholder="Phone"
            ></b-form-input>
          </b-form-group>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Email"
            label-for="email"
            label-align-sm="left"
          >
            <b-form-input
              id="email"
              v-model="form.orgCommunication.email"
              placeholder="email"
            ></b-form-input>
          </b-form-group>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Website link"
            label-for="website"
            label-align-sm="left"
          >
            <b-form-input
              id="website"
              v-model="form.organization.url"
              placeholder="www.rainforest.com"
              required
            ></b-form-input>
          </b-form-group>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="6"
            label="Organization Description"
            label-for="description"
            label-align-sm="left"
            id="desc"
          >
            <b-form-textarea
              id="description"
              v-model="form.organization.description"
              placeholder="Please describe what this Organization is all about..."
              rows="3"
              max-rows="6"
            ></b-form-textarea>
          </b-form-group>
          <b-tooltip
            target="desc__BV_label_"
            triggers="hover"
            variant="secondary"
            placement="lefttop"
          >
            Please describe the Organizaiton in around 500 words approx.
          </b-tooltip>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Key SDGs"
            label-for="keySDGs"
            label-align-sm="left"
          >
            <b-form-input
              id="keySDGs"
              v-model="form.orgSustainability.sdgs"
              placeholder="Key"
            ></b-form-input>
          </b-form-group>
          <br />
          <b-form-group
            label-cols="4"
            label-cols-lg="6"
            class="title"
            label="Target Audience"
            label-for="targetAudience"
            label-align-sm="left"
            id="prio"
          >
            <b-form-select
              id="priority"
              v-model="form.targetAudience"
              :options="targetAudienceOptions"
            ></b-form-select>
          </b-form-group>
          <br />
          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Applies To"
            label-for="appliesTo"
            label-align-sm="left"
          >
            <b-form-input
              id="name"
              v-model="appliesTo"
              placeholder="Applies To"
            ></b-form-input>
          </b-form-group>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Social Media"
            label-for="socialMedia"
            label-align-sm="left"
          >
            <b-form-input
              class="socialMediaInput"
              id="facebook"
              v-model="form.orgCommunication.facebookUrl"
              placeholder="Facebook"
            ></b-form-input>
            <b-form-input
              class="socialMediaInput"
              id="Twitter"
              v-model="form.orgCommunication.twitterUrl"
              placeholder="Twitter"
            ></b-form-input>
            <b-form-input
              class="socialMediaInput"
              id="Instagram"
              v-model="form.orgCommunication.instagramUrl"
              placeholder="Instagram"
            ></b-form-input>
          </b-form-group>
          <br />

          <b-form-group
            class="title"
            label-cols="4"
            label-cols-lg="3"
            label="Video"
            label-for="video"
            label-align-sm="left"
          >
            <b-form-input
              id="video"
              v-model="form.orgCommunication.videoUrl"
              placeholder="video"
            ></b-form-input>
          </b-form-group>

          <b-row class="buttons_row">
            <b-button class="actButton" type="reset">Reset</b-button>
            <b-button class="actButton" type="submit">Save</b-button>
          </b-row>
        </b-form>
      </b-col>
      <b-col>
        <UserCard
          :cardTitle="'Profile Picture'"
          :profilePic="form.organization.logoUrl"
          @url="changeLogoUrl"
          @file="saveLogoFile"
        />
      </b-col>
    </b-row>
  </div>
</template>

<script>
import SubmitMixin from "@/mixins/SubmitMixin";
//import { ProfilePicture } from "uicomponents"
import UserCard from "../Shared/UserCard";

export default {
  name: "UserForm",
  data() {
    return {
      form: {},
      targetAudienceOptions: [
        { text: "B2B", value: 0 },
        { text: "B2C", value: 1 },
      ],
    };
  },
  methods: {
    onReset() {
      this.$store.dispatch("resetOrganization");
    },
    async onSubmit(event) {
      event.preventDefault();
      await this.$store.dispatch("org/updateOrganization", this.form);
    },
    addNew() {
      this.onReset();
      this.$refs["proceed-modal"].hide();
    },
    changeLogoUrl(url) {
      this.form.logo = url;
    },
    saveLogoFile(logoFile) {
      this.logoFile = logoFile;
    },
  },
  mixins: [SubmitMixin],
  components: {
    UserCard,
  },
  async mounted() {
    this.form = await this.$store.dispatch("org/fetchOrg");
  },
};
</script>

<style scoped>
.editMain {
  width: 85%;
  min-height: 55vh;
}
.sectionTitle {
  text-align: left;
}
h2 {
  text-align: left;
  font-size: 20px;
  font-weight: 600;
}
#rating {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left !important;
}

#rating {
  margin-bottom: 10px !important;
}
.actButton {
  color: black;
  border: 2px solid #989898;
  background: white;
  font-weight: bold;
}
.title {
  font-size: 20px;
  font-weight: 600;
  display: grid;
  text-transform: uppercase;
}
.socialMediaInput {
  margin-bottom: 15px;
}
hr {
  margin-top: 0;
  padding-top: 0;
  height: 3px;
  background-color: #ebebeb;
  border: none;
}
</style>