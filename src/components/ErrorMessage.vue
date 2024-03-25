<template>
    <span>Para continuar, seleccione una opción.</span>
</template>
  
<script>
  export default {
    name: 'ErrorMessage',
   
}
</script>

<style scoped>
span{
    display: block;
    background-color: #FFDD57;
    width: auto;
    padding: 20px 40px;
    border-radius: 10px;
    margin-top: 20px;
    margin-right: 400px;
}
@media screen and (max-width: 450px) {
    span{
        margin-right: 0px;
    }
}
</style>