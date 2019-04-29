<template>
  <v-app>
    <v-toolbar dark color="primary">
  
      <v-toolbar-title class="white--text">進数変換<v-icon>swap_horiz</v-icon></v-toolbar-title>
  
      <v-spacer></v-spacer>
  
      <v-toolbar-items>
        <v-btn flat href="https://twitter.com/daniel_program">Twitter</v-btn>
        <v-btn flat href="https://github.com/pineapplehunter/base-converter">Github</v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-content>
      <BaseConverter />
    </v-content>
    <v-footer dark color="primary">
      <v-spacer />
      <span id="author">made by <a href="https://twitter.com/daniel_program" id="twitter-id"><b>@daniel_program</b></a></span>
    </v-footer>
  </v-app>
</template>

<script>
import BaseConverter from './components/BaseConverter.vue'

export default {
  name: 'app',
  components: {
    BaseConverter
  }
}
</script>

<style scoped>
#twitter-id {
  color: white;
}

#author {
  margin: 10px;
}
</style>

