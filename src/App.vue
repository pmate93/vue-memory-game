<template>

<Header 
:formattedElapsedTime="formattedElapsedTime"
@restart="restart"
@updateRange="updateRange" />

<div class="card-container">
  <div v-if="!isLoaded" class="loading-container">
    <img :src="loading" class="loading">
    <br>
    Waiting for cards to load...
  </div>
 <Card v-else v-for="card in cards" :key="card.id"  
 :cardUrl="card.url"
 :flipped="card.flipped"
 @click="isClicked(card.id)" />
</div>
<PopUp 
:modal="modal" 
@changeModal="changeModal"
:formattedElapsedTime="formattedElapsedTime"
/>

</template>

<script>
import Card from './components/Card.vue';
import PopUp from './components/PopUp.vue';

import loading from './assets/loading.gif';
import Header from './components/Header.vue';


export default {
  name: 'App',
  
  components: {
    Card,
    PopUp,
    Header
  },

  data(){
    return{
      loading: loading,
      isLoaded: false,
      elapsedTime: 0,
      timer: undefined,
      range: 12,
      numberOfCards: 0,

      cards: [
        {id: 0},
        {value: 0},
        {url: ''},
        {flipped: false}
      ],

      picUrls: [],

      previousCards: {
        first: -1,
        second: -1
      },

      clickLimiter: false,

      foundCards: [],
      modal: false
    }
  },

  methods:{
    updateRange(rangeOf){
      this.range = parseInt(rangeOf);
    },

    changeModal(){
      this.modal = false;
    },

    restart(){
      this.generateCards();
      this.getPictures();
    },

    generateCards(){
      this.cards = [];
      this.modal = false;
      this.foundCards = [];
      this.previousCards.first = -1;
      this.previousCards.second = -1;
      let arr = [];
      

      if(this.range % 2 === 0){
        this.numberOfCards = this.range;
      }else{
        this.numberOfCards = this.range - 1;
      }

      while(arr.length < this.numberOfCards){
        let duplicate = false;
        let counter = 0;
        let num = Math.floor(Math.random() * this.numberOfCards / 2);
        for(let i = 0; i < arr.length;i++){
          if(arr[i]===num){
            counter++;
            if(counter === 2){
              duplicate = true;
              break;
            }
          }
        }
        if(!duplicate){
          arr.push(num);
        }
      }
      for (let i = 0; i < arr.length; i++) {
        this.cards.push({
          value: arr[i],
          id: i,
          flipped: false
        })
      }
    },

    isClicked(id){

      if(!this.clickLimiter){

        for(let i = 0; i < this.foundCards.length; i++){
          if(this.foundCards[i] === id){
            return;
          }
        }

        if(this.previousCards.first === -1){
          this.previousCards.first = id;
        }else if(this.previousCards.second === -1 && this.previousCards.first !== id){
          this.previousCards.second = id;
        }
  
        for (let i = 0; i < this.cards.length; i++) {
          if(id === this.cards[i].id){
            if(!this.cards[i].flipped){
              this.cards[i].flipped = true;
              break;
            }
          }
        }
  
        let counter = 0;
        for (let i = 0; i < this.cards.length; i++) {
          if(this.cards[i].flipped){
            counter++;
          }
        }

        if(counter % 2 === 0){
          this.clickLimiter = true;
          if(this.cards[this.previousCards.first].value!==this.cards[this.previousCards.second].value){
            setTimeout(()=>{
              if (typeof this.cards[this.previousCards.first] !== 'undefined') {
                this.cards[this.previousCards.first].flipped = false;
                this.cards[this.previousCards.second].flipped = false;
                this.previousCards.first = -1;
                this.previousCards.second = -1;
                this.clickLimiter = false;
              }else{
                this.clickLimiter = false;
              }
            }, 1000)
          }else{
            setTimeout(()=>{
              this.foundCards.push(this.previousCards.first, this.previousCards.second);
              this.previousCards.first = -1;
              this.previousCards.second = -1;
              this.clickLimiter = false;

              if(this.foundCards.length === this.numberOfCards){
                this.modal = true;
                clearInterval(this.timer);
              }

            }, 1000)
          }
        }

      }

    },

    async getPictures(){
      this.isLoaded = false;
      this.elapsedTime = 0;
      this.picUrls = [];
      clearInterval(this.timer);

      for(let i = 0 ; i < this.numberOfCards / 2 ; i++){
      
        await fetch(`https://source.unsplash.com/random?sig=${i}`)
        .then(res =>{
          this.picUrls.push(res.url);
        })
      }
      this.isLoaded = true;
      for (let i = 0; i < this.numberOfCards / 2; i++) {
        for (let j = 0; j < this.cards.length; j++) {
          if(this.cards[j].value === i){
            this.cards[j].url = this.picUrls[i];
          }
        }

      }
      this.timer = setInterval(() => {
        this.elapsedTime += 1000;
      }, 1000);
    }
  },

  computed: {
    formattedElapsedTime() {
      const date = new Date(null);
      date.setSeconds(this.elapsedTime / 1000);
      const utc = date.toUTCString();
      return utc.substr(utc.indexOf(":") - 2, 8);
    }
  },

  created(){
    this.generateCards();
    this.getPictures();
  }
}
</script>

<style>
*{
  background-color: ivory;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.card-container{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  max-width:60rem;
  margin:auto;
}

.front:hover{
  opacity: 0.9;
}

.loading-container{
  text-align: center;
}

.loading{
  width:5rem;
  height: 5rem;
  margin-top: 11rem;
}

</style>