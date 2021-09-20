<template>
<div>{{this.cardNumber}}</div>
</template>

<script>


export default {
  name: 'Card',
  props:{
      cardNumber: Number
  },
  components: {

  },

  data(){
    return{

    }
  },

  methods: {

  }
}
</script>

<style scoped>

div{
    border: 1px solid black;
    height:2.5rem;
    width: 5rem;
    margin:3%;
    padding:3%;
    cursor: pointer;
}


</style>