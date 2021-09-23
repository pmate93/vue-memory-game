<template>
    <transition name="fade" appear>
    <div class="modal-overlay" v-if="showModal" @click="$emit('changeModal')"></div>
    </transition>
    <transition name="slide" appear>
    <div class="modal" v-if="showModal">
    <h1>Congratulations, You Won!</h1>
    <button class="button" @click="$emit('changeModal')">
        Close
    </button>
    </div>
    </transition>
</template>

<script>

  /* component idea comes from Tyler Potts https://codepen.io/Tyler_Potts_/pen/PowdPRQ */

export default {
    name:'PopUp',
    emits: ['changeModal'],
    props:{
      modal: Boolean
    },

    computed:{
      showModal:{
        get() {return this.modal},
        /* set(value) {
        // some logic
        this.$emit('update:name1', value) */
      }
    },

    data () {
        return {
        }
    }
}
</script>

<style scoped>
* {
 margin: 0;
 padding: 0;
 box-sizing: border-box;
}

.button {
 appearance: none;
 outline: none;
 border: none;
 background: none;
 cursor: pointer;
 
 display: inline-block;
 padding: 15px 25px;
 background-image: linear-gradient(to right, #2e9b40, #2dd465);
 border-radius: 8px;
 
 color: #FFF;
 font-size: 18px;
 font-weight: 700;
 
 box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
 transition: 0.4s ease-out;
}
 
button:hover {
  box-shadow: 6px 6px rgba(0, 0, 0, 0.6);
}


.modal-overlay {
 position: absolute;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 z-index: 98;
 background-color: rgba(0, 0, 0, 0.3);
}

.modal {
 position: fixed;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
 z-index: 99;
 
 width: 100%;
 max-width: 400px;
 background-color: #FFF;
 border-radius: 16px;
 
 padding: 25px;
 
}

h1 {
  color: #222;
  font-size: 32px;
  font-weight: 900;
  margin-bottom: 15px;
}
 
p{
  color: #666;
  font-size: 18px;
  font-weight: 400;
  margin-bottom: 15px;
}

.fade-enter-active,
.fade-leave-active {
 transition: opacity .5s;
}

.fade-enter,
.fade-leave-to {
 opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
 transition: transform .5s;
}

.slide-enter,
.slide-leave-to {
 transform: translateY(-50%) translateX(100vw);
} 

</style>