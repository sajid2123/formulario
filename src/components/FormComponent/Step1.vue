<template>
    <!-- question-1 -->
    <div class="form-input">
            <label for="" class="form-label">¿Cuentas con la ayuda pública o contratas de forma privada?</label>
            <!-- option 1 -->
            <div class="form-radio-input">
                <input type="radio" id="question-1-option-1" name="question-1" value="public" v-model="selectedOption">
                <label for="question-1-option-1" class="form-radio-label">Ayuda Pública</label><br>
            </div>
            <!-- option 2 -->
            <div class="form-radio-input">
                <input type="radio" id="question-1-option-2" name="question-1" value="privada" v-model="selectedOption">
                <label for="question-1-option-2" class="form-radio-label">Contratación Privada</label><br>
            </div>
        </div>
</template>
  
<script>
  export default {
    name: 'Step1',
    data() {
    return {
      selectedOption: null,
    };
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