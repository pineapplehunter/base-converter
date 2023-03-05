<template>
  <v-app>
    <v-app-bar dark color="primary" title="進数変換">
      <v-icon>swap_horiz</v-icon>

      <v-spacer></v-spacer>

      <v-toolbar-items>
        <v-btn flat href="https://twitter.com/daniel_program">Twitter</v-btn>
        <v-btn flat href="https://github.com/pineapplehunter/base-converter">Github</v-btn>
      </v-toolbar-items></v-app-bar>

    <v-main>
      <BaseConverter />
    </v-main>

    <v-footer dark color="primary" app>
      <v-spacer />
      <span id="author">made by
        <a href="https://twitter.com/daniel_program" id="twitter-id"><b>@daniel_program</b></a></span>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import BaseConverter from "./components/BaseConverter.vue";

export default {
  name: "app",
  components: {
    BaseConverter,
  },
};
</script>

<style scoped>
#twitter-id {
  color: white;
}

#author {
  margin: 10px;
}
</style>
