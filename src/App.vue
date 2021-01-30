<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="align-center">
       <span>INVESTIGATION OF THE EMISSION SPECTRA USING A SPECTROSCOPE</span>
      </div>
    </v-app-bar>

    <v-main>
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld
  },

  data: () => ({
    //
  })
};
</script>
