<template>
  <v-card
    class="mx-auto"
    max-width="800"
    max-height="800"
    elevation="10"
  >
    <v-img
      src="../assets/Spectrograph.png"
    ></v-img>
    <v-img
      src="../assets/graduation.png"
    ></v-img>

       <v-card-text>
      <v-row
        class="mb-0"
        justify="space-between"
      >
        <v-col class="text-left">
        <v-img
           src="../assets/spectroscop_1.jpg"
        ></v-img>
        </v-col>
      </v-row>
    </v-card-text>

    <v-toolbar
      flat
    >
      <v-toolbar-title>
        <span class="subheading">Spectroscope value: </span> <span  class="display-1 font-weight-light"
            v-text="calculateScale"
          ></span>
        
      </v-toolbar-title>
      <v-spacer></v-spacer>
      
    </v-toolbar>
    
         
    <v-card-text>
      <v-toolbar
      flat
      >
      <v-toolbar-title>
        <span class="subheading">Linear scale: </span>
        
      </v-toolbar-title>
      <v-spacer></v-spacer>
      
      </v-toolbar>
       <v-slider
        v-model="linearScale"
        track-color="grey"
        always-dirty
        step="1"
        :min="minLinearScale"
        :max="maxLinearScale"
        :thumb-size="24"
        thumb-label="always"
      >
          
      </v-slider>
       </v-card-text>

    <v-card-text>
        <v-toolbar
        flat
        >
          <v-toolbar-title>
            <span class="subheading">Circular scale: </span>

          </v-toolbar-title>
          <v-spacer></v-spacer>

        </v-toolbar>
       <v-slider
        v-model="circularScale"
        track-color="grey"
        always-dirty
        step="1"
        :min="minCircularScale"
        :max="maxCircularScale"
        :thumb-size="24"
        thumb-label="always"
      >
       
      </v-slider>
    </v-card-text>
 
    <v-card-text>
      <v-row align="center">
       
      <v-col
        class="d-flex"
        cols="12"
        sm="6"
      >
        <v-select
          v-model="gas"
          :items="specters"
          item-text="name"
          item-value="icon"
          label="Select gas..."
          outlined
        ></v-select>
      </v-col>

      
    </v-row>
    </v-card-text>


    

    <v-card-text>
      <v-row
        class="mb-4"
        justify="space-between"
      >
        <v-col class="text-left">
          <span
            class="display-3 font-weight-light"
            v-text="calculateWl"
          ></span>
          <span class="subheading font-weight-light mr-1">nm</span>
          <v-fade-transition>
            <v-avatar
              v-if="isPlaying"
              :color="color"
              :style="{
                animationDuration: animationDuration
              }"
              class="mb-1 v-avatar--metronome"
              size="12"
            ></v-avatar>
          </v-fade-transition>
        </v-col>
        <v-col class="text-right">
        </v-col>
      </v-row>

      <v-slider
        v-model="calculateWl"
        :color="color"
        track-color="grey"
        readonly
        always-dirty
        :min="minWl"
        :max="maxWl"
        :thumb-size="1"
        thumb-label="always"
      >
          <template v-slot:thumb-label>
            <v-img width="20px"
              :z-index="1"
              src="../assets/Arrow.png"
            ></v-img>
            
          </template>
      </v-slider>

      <v-img
      :width="770"
      :height="170"
      :src="gas"
      style="z-index: -1"
      ></v-img>
    </v-card-text>


  </v-card>
</template>
<script>
  import vuePlayer  from  '@algoz098/vue-player'  
  export default {
    name: "HelloWorld",
    components: {
			vuePlayer
		},
    data: () => ({
      gas: null,
      bpm: 0,
      zIndex: -1,
      linearScale: 6,
      maxLinearScale: 6,
      minLinearScale: 1,
      circularScale: 20,
      minCircularScale: 0,
      maxCircularScale: 50,
      minWl: 380,
      maxWl: 750,
      interval: null,
      isPlaying: false,
      spectroscopeScale: 3,
      imgSrc:"https://i.imgur.com/b0dnEv9.png",
      specters: [
        {name:"Hydrogen",icon:"https://i.imgur.com/WuqRrD7.png"},
        {name:"Neon",icon:"https://i.imgur.com/UMplp82.png"},
      ]
    }),

    computed: {
      color () {
        if (this.bpm >= 380 && this.bpm <= 450) return 'purple'
        if (this.bpm > 450 && this.bpm <= 485) return 'blue'
        if (this.bpm > 485 && this.bpm <= 500) return 'cyan'
        if (this.bpm > 500 && this.bpm <= 565) return 'green'
        if (this.bpm > 565 && this.bpm <= 590) return 'yellow'
        if (this.bpm > 590 && this.bpm <= 625) return 'orange'
        if (this.bpm > 625 && this.bpm <= 750) return 'red'
        return 'red'
      },
      calculateWl (){
        return Math.round(((this.circularScale*0.02+this.linearScale)/(this.maxLinearScale+this.maxCircularScale*0.02)) * this.maxWl)
      },
      calculateScale (){
        return this.circularScale*0.02+this.linearScale
      },
      animationDuration () {
        return `${60 / this.bpm}s`
      },
    },

    methods: {
      decrement () {
        this.bpm--
      },
      increment () {
        this.bpm++
      },
      toggle () {
        this.isPlaying = !this.isPlaying
      },
    },
  }
</script>