<template>
  <b-container class="signup_main" fluid="md">
    <img class="impakterLogo" src="@/assets/logo_index.png"/>

    <h1>Sign-up</h1>

    <h4>Contact info</h4>
    <b-alert
        :show="toggleStatusMessage"
        @dismissed="toggleStatusMessage = false"
        variant="danger"
        dismissible
        fade
    >{{ statusMessage }}
    </b-alert
    >

    <b-form @submit="onSubmit">
      <b-form-input
          class="identifier"
          id="company"
          v-model="userObj.organization.name"
          placeholder="Company Name"
          required
      >
      </b-form-input>

      <b-form-input
          class="identifier"
          v-model="userObj.userInformation.roleInOrg"
          placeholder="Your Role"
      >
      </b-form-input>
      <b-button class="action_btt" type="submit" variant="primary"
      >Sign Up
      </b-button
      >
    </b-form>
  </b-container>
</template>

<script>
import AccountMixin from "@/mixins/AccountMixin";
import CommonMixin from "@/mixins/CommonMixin";


export default {
  name: "SignUp-Part2",
  data() {
    return {
      company: "",
    };
  },
  mixins: [AccountMixin, CommonMixin],
  async mounted() {
    this.orgs = await this.$store.dispatch(
        "org/fetchOrgs"
    );
  },
  methods: {
    async onSubmit(event) {
      event.preventDefault();
      this.request.data = this.userObj;
      this.response = await this.$store.dispatch(
          "account/signup",
          this.request
      );
      let responseStatus = this.response.status;
      if (responseStatus.case === this.signupCases.SUCCESS) {
        let responseData = this.response.data;
        if (responseData.accessToken) {
          this.$store.commit("global/toggleLoading", "on");
          await this.$store.dispatch("account/afterLoginOrSignUp", this.response);
          this.$store.commit("global/toggleLoading", "off");
          await this.$router.push("/dashboard");
        }
      }
      else if (responseStatus.case === this.signupCases.FAILED) {
        this.statusMessage = responseStatus.message;
        this.toggleStatusMessage = true;
      }
    },
  },
};
</script>

<style scoped>
h1 {
  padding: 10px;
  font-weight: bold;
}

hr {
  flex: 1;
  height: 1px;
  background-color: #bbbbbb;
}

a:link {
  font-weight: bold;
  color: black;
  text-decoration: none;
}

.identifier {
  margin-top: 10px;
  margin-bottom: 10px;
}

.signup_main {
  margin-top: 10px;
  max-width: 500px;
}

.impakterLogo {
  margin-left: 40px;
  padding: 10px;
  max-width: 300px;
}

.action_btt {
  display: inline-block;
  background: white;
  color: #1d2029;
  width: 100%;
  border-radius: 5px;
  border: 2px solid #989898;
  white-space: nowrap;
  font-weight: bold;
}
</style>