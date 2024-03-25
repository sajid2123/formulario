<template>
    <div class="nav-steps">
      <ul>
        <li :class="[step === 'Step1' ? 'active' : '']">1</li>
        <li :class="[step === 'Step2' ? 'active' : '']">2</li>
        <li :class="[step === 'Step3' ? 'active' : '']">3</li>
        <li :class="[step === 'Step4' ? 'active' : '']">4</li>
      </ul>
    </div>
</template>
  
<script>
  export default {
    name: 'Nav',
    props: {
      step: String
    },
    data()
    {
        return {
            
        }
    }
}
</script>

<style scoped>
.nav-steps{
    display: flex;
    justify-content: center;
    padding: 20px;
    width: 100%;
    margin-top: 15px;
}
ul{
    display: flex;
    list-style: none;
}
li{
    margin-left: 120px;
    font-size: 35px;
    font-weight: bold;
    padding: 2px 20px 2px 20px;
    color: #0C2E69;
    background-color: #CBDCE5;
    border-radius: 50px;
}
.active{
    background-color: #0C2E69;
    color: white;
}
@media screen and (max-width: 450px) {
    .nav-steps{
    /* display: flex;
    justify-content: center; */
    padding: 20px !important;
    width: 100%;
    /* margin-top: 15px; */
    }
    ul{
        display: flex;
        list-style: none;
    }
    li{
        margin-left: 30px;
        font-size: 25px;
        font-weight: bold;
        padding: 2px 15px 2px 15px;
        color: #0C2E69;
        background-color: #CBDCE5;
        border-radius: 50px;
    }
    .active{
        background-color: #364f7b;
        color: white;
    }
}

</style>