<template>
  <div>
    <compHeader />
    <div class="resetPasswordContent">
      <div class="row">
        <div class="column1">
          <div>
            <div class="title">New password</div>
          </div>
          <form>
            <p></p>
            <div class="label">New password:</div>
            <br />
            <input
              v-model="password1"
              v-bind:class="{ 'incorrect': invalidPassword} "
              type="password"
              placeholder="Enter new password"
            />
            <div class="alert" v-bind:class="{success: changesuccessful}" v-if="showAlert">
              <span class="closebtn" @click="hideAlert">&times;</span>
              {{alertMessage}}
            </div>
            <p></p>
            <div class="label">Password</div>
            <br />
            <input
              v-model="password2"
              v-bind:class="{ 'incorrect': invalidPassword} "
              type="password"
              placeholder="Confirm your new password"
            />
            <br />
            <div class="btnLogin" @click="save">
              Save
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
import compHeader from "../components/compHeader.vue";

export default {
  name: "resetPassword",
  components: {
    compHeader
  },
  data() {
    return {
      password1: "",
      password2: "",
      alertMessage: "",
      showAlert: false,
      invalidPassword: false,
      changesuccessful: false
    };
  },
  mounted() {
    document.title = "Reset Password";
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
    save() {
      this.invalidPassword = false;
      this.changesuccessful = false;
      this.showAlert = false;
      if (this.password1.length < 6) {
        this.showAlert = true;
        this.alertMessage =
          "\u{000D7} Your password need to be at least 6 characters long.";
        this.invalidPassword = true;
        return;
      }
      if (this.password1 !== this.password2) {
        this.showAlert = true;
        this.alertMessage =
          "\u{000D7} Verify your password, it needs to be the same.";
        this.invalidPassword = true;
        return;
      }

      /*{
    "email": "eve.holt@reqres.in",
    "password": "pistol"
}*/
      this.postData("https://reqres.in/api/register", {
        email: "eve.holt@reqres.in",
        password: this.password1 //cityslicka
      })
        .then(data => {
          this.showAlert = true;
          this.changesuccessful = false;
          if (data.error != null) {
            this.alertMessage = data.error;
            this.changesuccessful = false;
            return; // don't proceed if an error was encountred
          }

          if (data.token == null) {
            this.alertMessage =
              "It was not possible to recover the token, try again.";
          } else {
            this.alertMessage =
              "Your password has been changed, you now will be redirected to login page.";
            this.changesuccessful = true;
            setTimeout(() => {
              window.location.href = "/";
            }, 1000);
          }
        })
        .catch(error => {
          this.changesuccessful = false;
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
@import "./resetPassword.scss";
</style>
