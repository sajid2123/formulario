<template>
    <button v-on:click="$emit('siguiente')">Siguiente</button>
</template>
  
<script>
  export default {
    name: 'Button',
}
</script>

<style scoped>
button{
    margin-left: 100px;  
    margin-top: 30px;
    padding: 10px 50px;
    color: white;
    background-color: #0C2E69;
    font-size: 20px;
    margin-bottom: 10px;
    border-radius: 30px;
    border: none;
    cursor: pointer;
}
@media screen and (max-width: 450px) {
    button{
        margin-left: 15px;
        padding: 10px 30px;
        font-size: 15px;
    }
}
</style>