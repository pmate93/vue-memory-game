<template>
<div class="head-container">
    <div class="title">
        <h1>Memory Game</h1>
        <div>
            with random images
        </div>
    </div>
    <div class="header">
        <h1>{{formattedElapsedTime}}</h1>
        <Restart 
        @click="$emit('restart')"
        />
    </div>
    <div class="range">
        <div class="range-title">
            change the number of cards
            <br>
            (then press restart)    
        </div>
    <input @change="$emit('updateRange', rangeOf)" class="range-input" type="range" name="range" v-model="rangeOf" min="4" max="20">
    </div>

</div>
  
</template>

<script>
import Restart from './Restart.vue';

export default {
    name: 'Header',
    emits:['restart', 'updateRange'],
    props: {
        formattedElapsedTime: String
    },

    components:{
        Restart
    },

    data(){
        return{
            rangeOf: 12
        }
    },
  
}
</script>

<style scoped>

.header{
    text-align: center;
    /* border: 1px solid black; */
    margin-bottom: 3%;
}

.head-container{
    display:flex;
    justify-content: center;
    margin-top: 60px;
}

.title{
    text-align: center;
    border-right: 3px solid black;
    margin-right: 1%;
    padding-right: 1%;
}

.title div{
    font-style: italic;
}

.range{
    border-left: 3px solid black;
    margin-left: 1%;
    padding-left: 1%;
    text-align: center;
}

.range-title{
    padding-top:15%;
    padding-bottom: 10%;
    font-style: italic;
}

.range-input{
    width: 90%;
}

@media screen and (max-width: 400px) {
    div{
        font-size: 80%;
    }
  
}

</style>