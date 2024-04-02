<template>
    <div class="form-input">
            <label for="" class="form-label">Las horas de servicio serán</label>
            <!-- option 1 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-1" name="question-3" value="week" v-model="options.selectedOption">
                <label for="question-3-option-1" class="form-radio-label">Solo entre semana</label><br>
            </div>
            <!-- option 2 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-2" name="question-3" value="weekend" v-model="options.selectedOption">
                <label for="question-3-option-2" class="form-radio-label">Domingos y / o festivos</label><br>
            </div>
            <!-- option 3 -->
            <div class="form-radio-input">
                <input type="radio" id="question-3-option-3" name="question-3" value="allWeek" v-model="options.selectedOption">
                <label for="question-3-option-3" class="form-radio-label">Toda la Semana</label><br>
            </div>
            <div class="checkbox-dias" v-if="options.selectedOption != null">
                    <h5 class="span-checkbox-dias">Selecciona los días</h5>
                    <div v-for="(dia) in dias">
                        <label  v-if="dia.name !== 'Dom' || options.selectedOption !== 'week'" class="input-checkbox-dias">
                            <input  
                            v-model="dia.checked" 
                            type=checkbox  
                            :disabled="options.selectedOption != 'week'"
                            ><br>
                            <span :class="{'checked': dia.checked }">{{dia.name}}</span>
                        </label>
                    </div>
                    
            </div>
            
            <div class="form-checkbox-input">
                <input type="checkbox" id="horas-nocturnas" name="horas-nocturnas" v-model="options.selectedCheckBox">
                <label for="horas-nocturnas" class="checkbox">Activar si hay horas nocturnas</label><br>
            </div>
    </div>
</template>
  
<script>
  export default {
    name: 'Step3',
    data() {
        return {
            options: {
                selectedOption: null,
                selectedDias: [],
                selectedCheckBox: false,
            },
            dias:[
                {
                name: 'Lun',
                checked: false},
                {
                name: 'Mar',
                checked: false},
                {
                name: 'Mie',
                checked: false},
                {
                name: 'Jue',
                checked: false},
                {
                name: 'Vie',
                checked: false},
                {
                name: 'Sab',
                checked: false},
                {
                name: 'Dom',
                checked: false},
            ]
        }
        
    },
    watch: {
        'options': {
          handler(newVal) {
              this.$emit('data', newVal);
          },
          deep: true
        },
        'options.selectedOption': function(newValue) {
            this.dias.forEach(dia => {
                if (newValue == 'allWeek') {
                    dia.checked = true;
                }else if (newValue == 'weekend' && dia.name == 'Dom') {
                    dia.checked = true;
                }else if (newValue == 'week' && dia.name != 'Dom') {
                    dia.checked = true;
                }else{
                    dia.checked = false;
                }
            });
        },
        'dias': {
            handler(newVal) {
                const selectedDays = [];
                newVal.forEach(dia => {
                    if(dia.checked == true){
                        switch (dia.name) {
                            case 'Lun':
                            selectedDays.push(1)
                                break;
                            case 'Mar':
                            selectedDays.push(2)
                                break
                            case 'Mie':
                            selectedDays.push(3)
                                break
                            case 'Jue':
                            selectedDays.push(4)
                                break
                            case 'Vie':
                            selectedDays.push(5)
                                break
                            case 'Sab':
                            selectedDays.push(6)
                                break
                            case 'Dom':
                            selectedDays.push(0)
                                break
                        }
                    }
                })
                this.options.selectedDias = selectedDays;
            },
            deep: true
        }
    },
}
</script>

<style scoped>
.span-checkbox-dias{
    color:#002E7B;
    font-size: 20px;
}
.checkbox-dias{
    margin-top: 20px;
}
.input-checkbox-dias{
    float: left;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #363636;
    height: 45px;
    width: 45px;
    margin-left: 2px;
}
.input-checkbox-dias span{
    padding: 0px 10px;
}
.checked{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 42px;
    width: 45px;
    border: 1px solid #3273DC;
    background-color: #3273DC;
    border-radius: 5px;
    color: white;

}
.input-checkbox-dias input[type="checkbox"] {
    appearance: none;
}
.input-checkbox-dias span{
    cursor: pointer;
}
</style>