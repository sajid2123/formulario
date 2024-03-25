<template>
    <div class="form-input">
            <label for="horas" class="form-label">¿Cuantas horas de servicio al día necesitas?</label>
            <div class="form-select">
                <select name="horas" id="horas"  class="form-custom-select">
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                    <option value="6">6</option>
                    <option value="7">7</option>
                    <option value="8">8</option>
                </select>
                <span>Horas</span>
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