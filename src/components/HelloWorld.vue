<template>
  <v-card
    class="mx-auto"
    max-width="100%"
    max-height="1450"
    
  >
  <v-card-text>
    <v-row align="center">
      <v-col
      class="d-flex"
        cols="12"
        sm="6">
        <v-img
      src="../assets/Spectrograph.png"
    ></v-img>
    
      </v-col>
      <v-col
        
        cols="1"
        sm="6">
        <v-select
          v-model="gas"
          :items="specters"
          item-text="name"
          item-value="media"
          label="Select gas..."
          outlined
        ></v-select>
        <v-card v-if="gas" class="d-flex">
        <v-img
            width="650px"
            height="150px"
            src="../assets/with-bulb.gif"
          ></v-img>
          <v-img
          width="200px"
          :src="gas.eye"
          
          ></v-img>
          </v-card>
      </v-col>
    </v-row>
  </v-card-text>

  <v-card-text>
    <v-row align="center">
       
    <v-col
      class="d-flex"
      cols="12"
      sm="6"
    >
        <v-img
      src="../assets/graduation.png"
      ></v-img>
      </v-col>
    <v-col
      class="d-flex"
      cols="12"
      sm="6"
    >
        <v-card-text>
      <v-toolbar
        
        flat
        >
          <v-toolbar-title>
            <span class="subheading">Linear scale: </span>
            
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-row align="center" height="200px" >
              <v-btn
              class="mx-2"
              fab
              dark
              small
              @click="decrementLinear"
              color="primary"
            >
              <v-icon dark>
                mdi-chevron-down
              </v-icon>
            </v-btn>
            
            <span class="subheading" v-text="linearScale"></span>
            <v-btn
              class="mx-2"
              fab
              dark
              small
              @click="incrementLinear"
              color="primary"
            >
              <v-icon dark>
                mdi-chevron-up
              </v-icon>
            </v-btn>
            </v-row>
        </v-toolbar>
       </v-card-text>

    <v-card-text>
        <v-toolbar
        
        flat
        >
          <v-toolbar-title>
            <span class="subheading">Circular scale: </span>
            
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-row align="center" height="200px" >
              <v-btn
              class="mx-2"
              fab
              dark
              small
              @click="decrementRadial"
              color="primary"
            >
              <v-icon dark>
                mdi-chevron-down
              </v-icon>
            </v-btn>
            
            <span class="subheading" v-text="circularScale"></span>
            <v-btn
              class="mx-2"
              fab
              dark
              small
              @click="incrementRadial"
              color="primary"
            >
              <v-icon dark>
                mdi-chevron-up
              </v-icon>
            </v-btn>
            </v-row>
        </v-toolbar>
       
    </v-card-text>
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

      <v-img v-if="gas"
      width="100%"
      :height="170"
      :src="gas.icon"
      style="z-index: -1"
      ></v-img>
    </v-card-text>


  </v-card>
</template>
<script>
  //import vuePlayer  from  '@algoz098/vue-player'  
  export default {
    name: "HelloWorld",
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
        {name:"Hydrogen", media:{icon:"https://i.imgur.com/WuqRrD7.png",eye:"https://i.imgur.com/y0JdoIU.png"}},
        {name:"Neon", media:{icon:"https://i.imgur.com/UMplp82.png",eye:"https://i.imgur.com/FjdMk5s.png"}},
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
      incrementLinear(){
        if(this.linearScale<this.maxLinearScale)
        {
            this.linearScale++;
        }
      },
      decrementLinear(){
        if(this.linearScale>this.minLinearScale)
        this.linearScale--;
      },
      incrementRadial(){
        if(this.circularScale<this.maxCircularScale)
        this.circularScale++;
      },
      decrementRadial(){
        if(this.circularScale>this.minCircularScale)
        this.circularScale--;
      },
      toggle () {
        this.isPlaying = !this.isPlaying
      },
      powerSwitch() {
        
        (".power").click(function(){
          ("body").toggleClass("active");
        });  
      
      }
    },
  }
</script>


