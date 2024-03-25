<template>
    <div class="form-input">
            <label for="" class="form-label">Las horas de servicio serán</label>
            <!-- option 1 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-1" name="question-3" value="week">
                <label for="question-3-option-1" class="form-radio-label">Solo entre semana</label><br>
            </div>
            <!-- option 2 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-2" name="question-3" value="weekend">
                <label for="question-3-option-2" class="form-radio-label">Domingos y / o festivos</label><br>
            </div>
            <!-- option 3 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-3" name="question-3" value="allWeek">
                <label for="question-3-option-3" class="form-radio-label">Toda la Semana</label><br>
            </div>
            <div class="form-checkbox-input">
                <input type="checkbox" id="horas-nocturnas" name="horas-nocturnas">
                <label for="horas-nocturnas" class="checkbox">Activar si hay horas nocturnas</label><br>
            </div>
    </div>
</template>
  
<script>
  export default {
    name: 'Step1',
}
</script>

<style scoped>

</style>