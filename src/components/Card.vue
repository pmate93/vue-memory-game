<template>
<div class="container">
  <vue-flip active-click="" >
            <template v-slot:front class="front">
              <img :src="this.background">
            </template>
            <template v-slot:back class="back">
              <img :src="this.cardUrl">
            </template>
  </vue-flip>

</div>

<!-- <img @click="isClicked" :src="this.clicked ? this.cardUrl : this.background" > -->
</template>

<script>
  import background from '../assets/bg.jpg';
  import VueFlip from 'vue-flip';


export default {

  name: 'Card',
  props:{
      cardNumber: Number,
      cardUrl: String
  },
  components: {
    'vue-flip': VueFlip
  },

  data(){
    return{
      clicked: false,
      background: background
    }
  },

  mounted(){
    
  },

  methods: {
    isClicked(){
      this.clicked = !this.clicked;
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
  object-fit:cover
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