<template>
  <div class="container">
    <vue-flip v-model="isFlipped"  width="auto" height="auto">
              <template v-slot:front class="front">
                <img :src="this.background">
              </template>
              <template v-slot:back class="back">
                <img :src="this.cardUrl">
              </template>
    </vue-flip>
  </div>
</template>

<script>
  import background from '../assets/bg.jpg';
  import VueFlip from 'vue-flip';


export default {

  name: 'Card',
  props:{
      cardUrl: String,
      flipped: Boolean
  },
  components: {
    'vue-flip': VueFlip
  },

  data(){
    return{
      background: background,
    }
  },

  mounted(){
    
  },

  methods: {

  },

  computed: {
    isFlipped:{
      get() {return this.flipped}
    }
  }
}
</script>

<style scoped>

img, .container{
  height:7rem;
  width: 11rem;
  margin:1%;
  padding:1%;
  cursor: pointer;
  object-fit:cover;
  border-radius: 10px;
}



@media screen and (max-width: 624px) {
  img, .container {
    height:4rem;
    width:8rem;

  }
}

@media screen and (max-width: 301px) {
  img, .container {
    height:3rem;
    width:6rem;

  }
} 
  



</style>