<template>
  <v-app :dark="setTheme">
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>gen</span>
        <span class="font-weight-light">FORM</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        icon
        href="https://github.com/Ashishkubehera"
        target="_blank"
        rel="noopener noreferrer"
      >
        <v-icon>mdi-human-greeting</v-icon>
      </v-btn>
      <v-btn
        icon
        href="https://github.com/Ashishkubehera/VuetiForm"
        target="_blank"
        rel="noopener noreferrer"
      >
        <v-icon>mdi-github</v-icon>
      </v-btn>
      <v-btn fab text @click="ActivateLightTheme = !ActivateLightTheme">
        <v-icon :color="`${!ActivateLightTheme && 'yellow'}`" :light="true"
          >mdi-brightness-4</v-icon
        >
      </v-btn>
    </v-app-bar>

    <v-content>
      <TheForm />
    </v-content>
  </v-app>
</template>

<script>
import TheForm from "./components/TheForm";

export default {
  name: "App",
  components: {
    TheForm
  },
  data: () => ({
    ActivateLightTheme: false
  }),
  computed: {
    setTheme() {
      if (this.ActivateLightTheme == true) {
        return (this.$vuetify.theme.dark = false);
      } else {
        return (this.$vuetify.theme.dark = true);
      }
    }
  }
};
</script>
<style>
html {
  overflow-y: auto !important;
}
</style>