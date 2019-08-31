<template>
  <div>
    <compHeader />
    <div class="signIn">
      <div class="row">
        <div class="column1">
          <div>
            <div class="title">Sign In</div>
          </div>
          <form>
            <p></p>
            <div class="label">Email:</div>
            <br />
            <input
              v-model="email"
              v-bind:class="{'incorrect': invalidEmail} "
              type="text"
              placeholder="Enter your Email"
            />
            <div class="alert" v-bind:class="{success: loginsuccessful}" v-if="showAlert">
              <span class="closebtn" @click="hideAlert">&times;</span>
              {{alertMessage}}
            </div>
            <p></p>
            <div class="label">Password</div>
            <br />
            <input
              v-model="password"
              v-bind:class="{ 'incorrect': invalidPassword} "
              type="password"
              placeholder="Enter password"
            />
            <br />
            <div class="resetPassword">
              <v-link href="/resetPassword">Want to reset password?</v-link>
            </div>
            <div class="btnLogin" @click="login">
              Login
              <i class="arrow right whiteArrowRight"></i>
            </div>
          </form>
        </div>
        <div class="column2">
          <img src="@/assets/login-shield.svg" alt="login-shield" class="image" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VLink from "../components/VLink.vue";
import compHeader from "../components/compHeader.vue";

export default {
  name: "signIn",
  components: {
    VLink,
    compHeader
  },
  data() {
    return {
      email: "",
      password: "",
      alertMessage: "",
      showAlert: false,
      invalidEmail: false,
      invalidPassword: false,
      loginsuccessful: false
    };
  },
  mounted() {
    document.title = "Sign In";
  },
  methods: {
    postData(url = "", data = {}) {
      return fetch(url, {
        method: "POST",
        mode: "cors",
        cache: "no-cache",
        credentials: "same-origin",
        headers: {
          "Content-Type": "application/json"
        },
        redirect: "follow",
        referrer: "no-referrer",
        body: JSON.stringify(data)
      }).then(response => response.json());
    },
    hideAlert() {
      this.showAlert = false;
    },
    login() {
      this.invalidEmail = false;
      this.invalidPassword = false;
      this.loginsuccessful = false;
      this.showAlert = false;
      this.postData("https://reqres.in/api/login", {
        email: this.email, //eve.holt@reqres.in
        password: this.password //cityslicka
      })
        .then(data => {
          this.showAlert = true;
          this.loginsuccessful = false;
          if (data.error != null) {
            switch (data.error) {
              case "Missing email or username":
                this.alertMessage =
                  "\u{000D7} Please check the username and password before login.";
                this.invalidEmail = true;
                break;
              case "Missing password":
                this.alertMessage =
                  "\u{000D7} Please insert your password before login.";
                this.invalidPassword = true;
                break;
              case "user not found":
                this.alertMessage = "\u{000D7} Please re-check your email.";
                this.invalidEmail = true;
                break;
              default:
                //untreated error
                this.alertMessage = data.error;
                this.loginsuccessful = true;
                this.invalidEmail = true;
            }
            return; // don't proceed if an error was encountred
          }

          if (data.token == null) {
            this.alertMessage =
              "It was not possible to recover the token, try again.";
          } else {
            this.alertMessage = "Your login was successful.";
            this.loginsuccessful = true;
          }
        })
        .catch(error => {
          this.loginsuccessful = false;
          this.showAlert = true;
          this.alertMessage = error.error;
        });
    },
    alerta: () => {
      this.showAlert = true;
    }
  }
};
</script>

<style lang="scss">
@import "../main.scss";
@import "./signIn.scss";
</style>
