<template>
    <div class="form-input">
            <label for="" class="form-label">¿Durante cuánto tiempo necesitas el servicio?</label>
            <!-- option 1 -->
            <div class="form-radio-input">
                <input type="radio" id="question-2-option-1" name="question-2" value="less" v-model="selectedOption">
                <label for="question-2-option-1" class="form-radio-label">Menos de un mes</label><br>
            </div>
            <!-- option 2 -->
            <div class="form-radio-input">
                <input type="radio" id="question-2-option-2" name="question-2" value="more" v-model="selectedOption">
                <label for="question-2-option-2" class="form-radio-label">Más de un mes</label><br>
            </div>
        </div>
</template>
  
<script>
  export default {
    name: 'Step2',
    data() {
        return {
            selectedOption: null,
        }
    },
    watch: {
        selectedOption(newVal) {
            this.$emit('data', newVal);
        },
    },
}
</script>

<style scoped>

</style>