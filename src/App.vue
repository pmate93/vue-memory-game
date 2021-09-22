<template>

<div class="card-container">
 <Card v-for="card in cards" :key="card.id"  
 :cardUrl="card.url"
 :flipped="card.flipped"
 @click="isClicked(card.id)" />
</div>


</template>

<script>
import Card from './components/Card.vue';

export default {
  name: 'App',
  components: {
    Card
  },

  data(){
    return{
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

      clickLimiter: false
    }
  },

  methods:{
    generateCards(){
      this.cards = [];
      let arr = [];

      while(arr.length < 12){
        let duplicate = false;
        let counter = 0;
        let num = Math.floor(Math.random() * 6);
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
        if(this.previousCards.first === -1){
          this.previousCards.first = id;
        }else if(this.previousCards.second === -1){
          this.previousCards.second = id;
        }
  
        for (let i = 0; i < this.cards.length; i++) {
          if(id === this.cards[i].id){
            this.cards[i].flipped = !this.cards[i].flipped;
            console.log(this.cards[i].flipped);
            break;
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
          setTimeout(()=>{
            this.cards[this.previousCards.first].flipped = false;
            this.cards[this.previousCards.second].flipped = false;
            this.previousCards.first = -1;
            this.previousCards.second = -1;
            this.clickLimiter = false;
          }, 1000)
        }

      }

    },

    async getPictures(){
      for(let i = 0 ;i < 6 ; i++){
      
      await fetch(`https://source.unsplash.com/random?sig=${i}`)
      .then(res =>{
        this.picUrls.push(res.url);
      })
    }
    for (let i = 0; i < 6; i++) {
      for (let j = 0; j < this.cards.length; j++) {
        if(this.cards[j].value === i){
          this.cards[j].url = this.picUrls[i];
        }
      }
      
    }
    console.log(this.cards);
    }
  },

  computed:{
  },

  created(){
    this.generateCards();
    this.getPictures();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}

.card-container{
  display: flex;
  justify-content: center;
  border: 1px solid black;
  flex-wrap: wrap;
  width:60rem
}
</style>