<template>
  <div class="compHeader">
    <div class="leftcontent">
      <v-link href="/">Frankie.</v-link>
    </div>
    <div class="rightcontent">
      <span class="about">About Frankie</span>
      <img src="@/assets/login-user.svg" alt="Avatar" class="avatar" />
      <i class="arrow down"></i>
    </div>
  </div>
</template>

<script>
import VLink from "./VLink.vue";
export default {
  name: "compHeader",
  components: {
    VLink
  }
};
</script>

<style lang="scss">
@import "../main.scss";
@import "./compHeader.scss";
</style>
