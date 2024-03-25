<template>
    <div>
        <Nav />
        <NavSteps :step="activeStep" v-if="activeStep != 'Step5'"/>
        <form action="" class="form-box">
            <Step1 v-if="activeStep === 'Step1'" @data="handleData('Step1', $event)"/>
            <Step2 v-if="activeStep === 'Step2'" @data="handleData('Step2', $event)"/>
            <Step3 v-if="activeStep === 'Step3'"/>
            <Step4 v-if="activeStep === 'Step4'"/>
            <Step5 v-if="activeStep === 'Step5'"/>
            <Error v-if="!isValid" />
        </form>
        <Button @siguiente="nextStep"  name="Siguiente"/>
    </div>
</template>
  
<script>
import Nav from './Nav.vue'
import NavSteps from './NavSteps.vue'
import Error from './ErrorMessage.vue'
import Button from './Button.vue'
import Step1 from './FormComponent/Step1.vue'
import Step2 from './FormComponent/Step2.vue'
import Step3 from './FormComponent/Step3.vue'
import Step4 from './FormComponent/Step4.vue'
import Step5 from './FormComponent/Step5.vue'

export default {
    name: 'Index',
    components: {
        Nav,
        NavSteps,
        Button,
        Step1,
        Step2,
        Step3,
        Step4,
        Step5,
        Error,
    },
    data()
    {
        return {
            activeStep: 'Step1',
            isValid: true,
            formData: {
                step1: null,
                step2: null,
                step3: {
                    respuesta: null,
                    dias: null,
                    horasNocturnas: null,
                },
                step4: null,
                step5: {
                    nombre: null,
                    correo: null,

                }
            },
        }
    },
    watch: {
        activeStep(newStep) {
            const Cookies = require('js-cookie');
            Cookies.set('activeStep', newStep);
        }
    },
    methods: {
        nextStep(){
            if(this.activeStep == 'Step1' && this.validateStep1()){
                this.isValid = true;
                this.activeStep = 'Step2';
            }else if(this.activeStep == 'Step2' && this.validateStep2()){
                this.isValid = true;
                this.activeStep = 'Step3';
            }else if(this.activeStep == 'Step3'){
                this.activeStep = 'Step4';
            }else if(this.activeStep == 'Step4'){
                this.activeStep = 'Step5';
            }
        },
        validateStep1(){
            let boolean = false;
            if(this.formData.step1 !== null) boolean = true;
            else this.isValid = false;
            return boolean;
        },
        validateStep2(){
            let boolean = false;
            if(this.formData.step2 !== null) boolean = true;
            else this.isValid = false;
            return boolean;
        },
        handleData(step, $event){
            switch(step){
                case 'Step1':
                    this.saveDataInCookies('Respuesta1', $event)
                    this.formData.step1 = $event;
                    break;
                case 'Step2':
                    this.saveDataInCookies('Respuesta2', $event)
                    this.formData.step2 = $event;
                    console.log(this.formData.step2);
                    break;
                case 'Step3':
                    this.saveDataInCookies('Respuesta2', $event)
                    break;
                case 'Step4':
                    this.saveDataInCookies('Respuesta2', $event)
                    break;
                case 'Step5':
                    this.saveDataInCookies('Respuesta2', $event)
                    break;
            }
        },
        saveDataInCookies(name,data){
            const Cookies = require('js-cookie');
            Cookies.set(name, data);
        }
    },
    mounted() {
        const Cookies = require('js-cookie');
        let activeStepValue = Cookies.get('activeStep');
        if (activeStepValue != undefined) {
            this.activeStep = activeStepValue;
        }
        let respuesta1 = Cookies.get('Respuesta1');
        if(respuesta1 != undefined){
            this.formData.step1 = respuesta1;
        }

        let respuesta2 = Cookies.get('Respuesta2');
        if(respuesta2 != undefined){
            this.formData.step2 = respuesta2;
        }

        console.log(this.formData);

    }
}
</script>

<style>
/* Form Style */
.form-box {
    margin-left: 100px;  
    margin-top: 30px;
}

.form-input {
    display: flex;
    flex-direction: column;
}
.checkbox{
    margin-left: 15px;
}
.form-label {
    color: #0C2E69;
    font-size: 30px;
    font-weight: bold;
}

.form-radio-input, .form-checkbox-input{
    margin-top: 15px;
}

input[type="radio"] {
    display: none;
}

input[type="radio"]:checked + .form-radio-label {
    display: inline-block;
    background-color: #FF595A;
    color: white;
    transition: .50s ease;
    border-radius: 20px;
    width: auto;
    padding: 5px 40px 5px 40px;
}

.form-radio-label {
    position: relative;
    padding-left: 40px; 
    font-size: 20px;
    cursor: pointer;
    color: #FF595A;
}

.form-radio-label:before {
    content: "";
    position: absolute;
    left: 0;
    top: 50%;
    transform: translateY(-50%);
    background-color: transparent;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    transition: .50s ease;
    box-shadow:  inset 0 0 0 1px #FF595A;
}

input[type="radio"]:checked + .form-radio-label:before {
    box-shadow: inset 0 0 0 10px #FF595A;
}
.form-custom-select {
    width: 80px;
    padding: 5px 15px;
    border: 2px solid #FF595A; 
    border-radius: 20px;
    background-color: white; 
    position: relative;
    font-size: 16px; 
    cursor: pointer; 
}
.form-select{
    display: flex;
}
.form-select span{
    margin-left: 20px;
    font-size: 25px;
    color: #FF595A;
}
@media screen and (max-width: 450px) {
    .form-box {
        margin: 30px 15px 0px 15px;
    }
    .form-label {
        color: #0C2E69;
        font-size: 20px;
        font-weight: bold;
    }
    .form-radio-label {
        padding-left: 20px; 
        font-size: 16px;
    }

    .form-radio-label:before {
        width: 15px;
        height: 15px;
    }
    .form-custom-select {
        width: 60px;
        padding: 2px 10px;
    }
    .form-select{
        display: flex;
    }
    .form-select span{
        margin-left: 20px;
        font-size: 18px;
        color: #FF595A;
    }
}
</style>