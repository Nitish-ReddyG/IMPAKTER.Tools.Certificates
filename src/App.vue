<template>
  <div id="app">
    <b-overlay className="loadSymbol" :show="loading" rounded="sm" opacity="0.30">
      <!--<Sidebar v-if="isInside" />-->
      <!-- <rise-loader :loading="isLoading" :color="color" :size="size"></rise-loader> -->
      <!-- <Spinner></Spinner> -->

      <b-row v-if="isLoggedIn">
        <NavDashboard/>
        <Sidebar/>
      </b-row>
      <b-row v-else>
        <NavigationBar/>
      </b-row>
      <b-row>
        <b-container>
          <b-alert
              v-if="showMessage"
              :variant="messageType"
              show
              dismissible
              fade
          >{{ message }}
          </b-alert
          >
        </b-container>
      </b-row>
      <b-row>
        <div id="router_view_container">
          <router-view/>
        </div>
      </b-row>
      <b-modal ref="message-modal" hide-footer>
        <p>{{ message }}</p>
        <b-button
            v-if="isMessageModalRedirectionButton"
            class="mt-3"
            block
            @click="redirectionButtonClicked"
        >{{ messageModalRedirectionText }}
        </b-button
        >
        <b-button class="mt-3" block @click="modalButtonClicked"
        >Go to Dashboard
        </b-button
        >
      </b-modal>
      <div v-if="isLoggedIn">
        <FooterDash/>

      </div>
      <div v-else>
        <Footer/>
      </div>
      <footer class="site-footer">
        <p>2021 Copyright © ImpakterLimited</p>
      </footer>
      <!--
            <footer class="site-footer">
              <p>2021 Copyright © ImpakterLimited</p>
            </footer> -->
    </b-overlay>
  </div>
</template>

<script>
import NavigationBar from "./components/Shared/NavigationBar";
//import DashBoardTabNav from "./components/Shared/DashBoardTabNav.vue";
import Sidebar from "./components/Shared/Sidebar";
import NavDashboard from "./components/Shared/NavDashboard";
import Footer from "./components/Shared/Footer";
import FooterDash from "./components/Dashboard/FooterDash";

//import PulseLoader from "vue-spinner/src/PulseLoader.vue";
//import RiseLoader from "vue-spinner/src/RiseLoader.vue";

//import Spinner from "vue-simple-spinner";

//import Loading from "vue-loading-overlay";
//import "vue-loading-overlay/dist/vue-loading.css";

export default {
  data() {
    return {
      isLoggedIn: false,
      messages: [],
      tabs: ["Home", "Hello"],
      loading: false,
      showMessage: false,
      messageType: "success",
      message: "hello there",
      messageModalRedirectionText: "Add another",
      isMessageModalRedirectionButton: false,
    };
  },
  methods: {
    modalButtonClicked() {
      this.$refs["message-modal"].hide();
      this.$router.push({name: "Dashboard"});
    },
    redirectionButtonClicked() {
      this.$refs["message-modal"].hide();
      this.$router.push({name: this.redirection});
    },
  },
  components: {
    NavigationBar,
    Sidebar,
    NavDashboard,
    Footer,
    FooterDash,
  },
  async mounted() {
    window.scrollTo(0, 0);
    this.isLoggedIn = await this.$store.dispatch("account/reloadApp");
    //this.$refs["message-modal"].show();
  },
  created() {
    document.title = "Impakter - Certificates";
    this.logInEventSubscription = this.$store.getters[
        "account/logInEvent"
        ].subscribe((message) => {
      console.log("message from subscription on App.vue:", message);
      if (message == "loggedIn") {
        this.isLoggedIn = true;
      }
      else {
        this.isLoggedIn = false;
      }
    });

    this.messageEventSubscription = this.$store.getters[
        "global/messageEvent"
        ].subscribe((payload) => {
      console.log(
          "message from message subscription on App.vue:",
          payload.message
      );
      if (payload.type == 0) {
        this.message = "Network failure: Please contact Administrator";
        this.$refs["message-modal"].show();
        //this.$alert("Network failure: Please contact Administrator");
      }
      else {
        this.message = payload.message;
        if (payload.redirection) {
          this.isMessageModalRedirectionButton = true;
          this.redirection = payload.redirection;
        }
        this.$refs["message-modal"].show();
        //this.$alert(payload.message);
      }
    });

    this.loadingEventSubscription = this.$store.getters[
        "global/loadingEvent"
        ].subscribe((payload) => {
      if (payload == "on") {
        this.loading = true;
      }
      else {
        this.loading = false;
      }
    });
  },
  beforeDestroy() {
    this.logInEventSubscription.unsubscribe();
    this.messageEventSubscription.unsubscribe();
    this.loadingEventSubscription.unsubscribe();
  },
};
</script>

<style lang="scss">
@import "assets/scss/custom-vars.scss";
@import "~bootstrap/scss/bootstrap.scss";
@import "~bootstrap-vue/src/index.scss";

* {
  font-family: "Montserrat";
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #010101;
}

.site-footer {
  border-top: solid 1px #BBBBBB;
  padding-top: 15px;
  justify-content: center;
  text-align: center;
  height: 50px;
  background: #f8f8f8;
}

button {
  margin-left: 10px;
}

#logo {
  width: 190px;
}

#refresh {
  color: rgb(79, 167, 162);
}

.main_row {
  margin-top: 0px;
}

.buttons_row {
  justify-content: center;
  margin-bottom: 10px;
  margin-top: 10px;
}

.buttons_row * {
  margin-right: 10px;
  margin-left: 10px;
}


#checkbox-group-1 {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left !important;
}

#input-group-1 {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left !important;
}

#input-group-2 {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left !important;
}

#router_view_container {
  // margin: 50px 50px 50px;
  width: 100%;
  margin: 50px 0 250px 400px;
}

.pagination {
  margin-top: 20px;
  margin-bottom: 40px;
}

.b-overlay {
  display: flex;
  align-content: center;
  justify-content: center;
  background-color: none;
}
</style>