<template>
    <div class="s-header-styling">
      <img src="../assets/logo-aiudo-cuidadoras-de-personas-mayores.svg" width="200">
    </div>
</template>
  
<script>
  export default {
    name: 'Nav',
    props: {
      msg: String
    }
}
</script>

<style scoped>
.s-header-styling{
    padding: 20px;
    width: 100%;
    box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.3);
}
img{
  margin-left: 100px;  
}
@media screen and (max-width: 450px) {
  .s-header-styling{
    display: flex;
    justify-content: center;
  }
  img{
   margin: 0px;
   width: 120px;
  }
}
</style>