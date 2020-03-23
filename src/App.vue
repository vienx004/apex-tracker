<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="ApexLegends Logo"
          class="shrink mr-2"
          contain
          src="../src/assets/apex-logo/apex-legends-symbol-white.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mr-2"
          contain
          min-width="80"
          src="../src/assets/apex-logo/apex-legends-logo-white.png"
          width="80"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <ApexTracker/>
    </v-content>
  </v-app>
</template>

<script>
import ApexTracker from './components/ApexTracker';

export default {
  name: 'App',

  components: {
    ApexTracker,
  },

  data: () => ({
    //
  }),
};
</script>
