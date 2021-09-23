<template>
  <button @click="$emit('restart')">Restart</button>
</template>

<script>
export default {
    name: 'Restart',
    emits: ['restart'],

}
</script>

<style scoped>

button {
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

</style>