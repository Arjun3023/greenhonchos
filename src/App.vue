<template>
  <v-app>
    <v-app-bar flat app color="white" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://www.greenhonchos.com/wp-content/uploads/2021/09/GH-logo-Copy.png"
          transition="scale-transition"
          width="200"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        color="success"
        href="https://www.greenhonchos.com/"
        target="_blank"
      >
        <span class="mr-2">Contact Us</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
    <v-divider />

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>

<style scoped>
.v-main {
  font-family: Avenir-Roman;
}
</style>
