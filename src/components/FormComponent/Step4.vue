<template>
    <div class="form-input">
            <label for="horas" class="form-label">¿Cuantas horas de servicio al día necesitas?</label>
            <div class="form-select">
                <select name="horas" id="horas"  class="form-custom-select" v-model="selectedOption"> 
                    <<option v-for="hora in horas" :key="hora" :value="hora">{{ hora }}</option>
                </select>
                <span>Horas</span>
            </div>
        </div>
</template>
  
<script>


  export default {
    name: 'Step1',
    props: {
        dias: Number,
    },
    data() {
        return {
            selectedOption: null,
            horas: [],
        }
    },
    watch: {
        selectedOption(newVal) {
            this.$emit('data', newVal);
        },
    },
    mounted(){
        const minimumHours = Math.ceil(16/this.dias);
        this.selectedOption = minimumHours;
        for(let i=minimumHours; i < 8; i++){
            this.horas.push(i);
        }
    }
}
</script>

<style scoped>

</style>