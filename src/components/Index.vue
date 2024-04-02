<template>
    <div>
        <Nav />
        <NavSteps :step="activeStep" v-if="activeStep != 'Step5'"/>
        <form @submit.prevent="handleSubmit" class="form-box">
            <Step1 
            v-if="activeStep === 'Step1'" 
            @data="handleData('Step1', $event)"
            />
            <Step2 
            v-if="activeStep === 'Step2'" 
            @data="handleData('Step2', $event)"
            />
            <Step3 
            v-if="activeStep === 'Step3'" 
            @data="handleData('Step3', $event)"
            />
            <Step4 
            v-if="activeStep === 'Step4'"
            @data="handleData('Step4', $event)"
            :dias="countDays()"
            />
            <Step5 
            v-if="activeStep === 'Step5'"
            @data="handleData('Step5', $event)"
            />
            <Error 
            v-if="isValid"
            />
            <button type="submit" class="submit"  v-if="activeStep == 'Step5'">ENVIAR PRESUPUESTO</button>
        </form>
        <Button @siguiente="nextStep"  name="Siguiente" v-if="activeStep != 'Step5'"/>
    </div>
</template>
  
<script>
import axios from 'axios'
import Nav from './Nav.vue'
import NavSteps from './NavSteps.vue'
import Error from './ErrorMessage.vue'
import Button from './Button.vue'
import Step1 from './FormComponent/Step1.vue'
import Step2 from './FormComponent/Step2.vue'
import Step3 from './FormComponent/Step3.vue'
import Step4 from './FormComponent/Step4.vue'
import Step5 from './FormComponent/Step5.vue'
import Swal from 'sweetalert2'

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
            isValid: false,
            formData: {
                step1: null,
                step2: null,
                step3: {
                    respuesta: null,
                    dias: null,
                    horasNocturnas: null,
                },
                step4: 1,
                step5: {
                    nombre: null,
                    correo: null,
                    telefono: null,
                    codigoPostal: null,
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
        async handleSubmit(){
            const endpoint = 'https://erp.aiudo.es/api/sad/funnel/submit';
            const payload = this.handlePayload();
            try {
                const response = await axios.post(endpoint, payload);
                // Handle success
                console.log(response.data);
                if (response.data.ok) {
                    Swal.fire({
                    title: 'SOLICITUD RECIBIDA',
                    text: 'Se ha enviado un correo a tu cuenta con los detalles de tu solicitud.',
                    icon: 'success',
                    allowOutsideClick: false,
                    confirmButtonText: 'Entendido',
                    customClass: {
                        confirmButton: 'confirm',
                        container: "containerPopup"
                    }
                    }).then((result) => {
                        if (result.value) {
                            window.location.href = '/'
                        }
                    })
                }else{
                    Swal.fire({
                        title: '<span class="cherrytext">SOLICITUD ERRÓNEA<span>',
                        text: 'Vuelve a intentarlo más tarde, disculpa las molestias.',
                        allowOutsideClick: false,
                        icon: 'error',
                        confirmButtonText: 'Entendido',
                        customClass: {
                            confirmButton: 'confirm',
                            container: "containerPopup"
                        }
                    }).then((result) => {
                        if (result.value) {
                            return (window.location.href = 'https://www.aiudo.es/sad')
                        }
                    })
                }

            } catch (error) {
                // Handle error
                console.error(error);
            }
           
        },
        handlePayload(){
            const name = this.formData.step5.nombre;
            const email = this.formData.step5.correo;
            const phone = this.formData.step5.telefono;
            const cp = this.formData.step5.codigoPostal;
            
            const customer = {
                name: name,
                email: email,
                phone: phone,
                cp: cp,
            }
            const Cookies = require('js-cookie');
            const hours = this.formData.step4;
            const week = this.formData.step3.respuesta;
            const duration = this.formData.step2;
            const help_type = this.formData.step1;
            const gclid = Cookies.get('gclid')
            const gaid = Cookies.get('gaid')
            const countDays =  this.countDays();
            const has_nightly_hours = this.formData.step3.horasNocturnas;

            //console.log("endpoint", endpoint);
            const payload = {
            // ...gaconnector,
            help_type: help_type,
            month_duration: duration,
            service_type: week,
            service_hours: parseInt(hours),
            gclid: gclid,
            gaid: gaid,
            // ga_client_id: gaconnector.gaconnector_GA_Client_ID,
            has_nightly_hours: has_nightly_hours,
            countDays: countDays,
            customer
            }


            return payload;
        },
        countDays(){
            const days = this.formData.step3.dias;
            const today = new Date();
            const tomorrow = new Date(today.getFullYear(), today.getMonth(), today.getDate() + 1);

            let nextMonth;
            const december = 11;
            if (today.getMonth() == december) {
                nextMonth = new Date(today.getFullYear() + 1, 0, today.getDate());
            } else {
                nextMonth = new Date(today.getFullYear(), today.getMonth() + 1, today.getDate());
            }
            const totalDays = Math.ceil((nextMonth - tomorrow) / (1000 * 60 * 60 * 24)) + 1;

            const startingDay = tomorrow;
            let especificWorkingDays = 0;
            for (let i = 0; i < totalDays; i++) {
                if (days.includes(startingDay.getDay())) {
                    especificWorkingDays++
                }
                startingDay.setDate(startingDay.getDate() + 1)
            }
            return especificWorkingDays;
        },
        nextStep(){
            if(this.activeStep == 'Step1' && this.validateStep(1)){
                this.activeStep = 'Step2';
                const data = this.formData.step1;
                this.saveDataInCookies('Respuesta1', data);
            }else if(this.activeStep == 'Step2' && this.validateStep(2)){
                this.activeStep = 'Step3';
                const data = this.formData.step2;
                this.saveDataInCookies('Respuesta2', data);
            }else if(this.activeStep == 'Step3' && this.validateStep(3)){
                this.activeStep = 'Step4';
                const data = this.formData.step3;
                this.saveDataInCookies('Respuesta3', data);
            }else if(this.activeStep == 'Step4' && this.validateStep(4)){
                this.activeStep = 'Step5';
                const data = this.formData.step4;
                this.saveDataInCookies('Respuesta4', data);
            }
            

        },
        errorMessage(action){
            switch(action){
                case 'show': 
                    this.isValid = true;
                    break;
                case 'hide':
                    this.isValid = false;
                    break;
            }
        },
       
        validateStep(stepNumber) {
            let isValid = false; 

            if (stepNumber === 1 || stepNumber === 2 || stepNumber === 4) {
                isValid = this.formData[`step${stepNumber}`] !== null;
            } else if (stepNumber === 3) {
                const { respuesta, dias } = this.formData.step3;
                isValid = respuesta !== null && dias !== null;
            }
            this.errorMessage(isValid ? 'hide' : 'show');
            return isValid;
        },
        handleData(step, $event){
            switch(step){
                case 'Step1':
                    this.formData.step1 = $event;
                    this.errorMessage('hide');
                    break;
                case 'Step2':
                    this.formData.step2 = $event;
                    this.errorMessage('hide');
                    break;
                case 'Step3':
                    this.formData.step3 = {
                        respuesta:  $event.selectedOption,
                        dias: $event.selectedDias,
                        horasNocturnas: $event.selectedCheckBox,
                    }
                    this.errorMessage('hide');
                    break;
                case 'Step4':
                    this.formData.step4 = $event;
                    this.errorMessage('hide');
                    break;
                case 'Step5':
                    this.formData.step5 = {
                        nombre: $event.nombre,
                        correo: $event.correo,
                        telefono: $event.telefono,
                        codigoPostal: $event.codigoPostal,
                    }
                    break;
            }
        },
        saveDataInCookies(name,data){
            if(name == 'Respuesta3'){
                const Cookies = require('js-cookie');
                const step3 = {
                    respuesta:  data.respuesta,
                    dias: data.dias,
                    horasNocturnas: data.horasNocturnas,
                }
                const step3String = JSON.stringify(step3);
                Cookies.set(name, step3String);
            }else{
                const Cookies = require('js-cookie');
                Cookies.set(name, data);
            }
        },
        onPopState: function (ev) {
            ev.preventDefault()
            console.log("ADSAd")
            history.pushState({}, 'AiUDO - Los mayores cuidados', '/funnel/' + this.currentStep.slug)

            Swal.fire({
                type: 'question',
                icon: "question",
                title: '¿Lo dejamos a medias?',
                text: 'Mejoramos cualquier presupuesto de la competencia.',
                confirmButtonText: 'OK, sigamos',
                cancelButtonText: 'Puedo esperar',
                showCancelButton: true,
                customClass: {
                    confirmButton: 'confirm',
                    container: "containerPopup",
                    cancelButton: 'cancel'
                },
            }).then(function (result) {
                if (!result.value) {
                    window.location.assign('https://ancient.aiudo.es/servicio-ayuda-a-domicilio')
                }
            })
        },
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

        let respuesta3 = Cookies.get('Respuesta3');
        if(respuesta3 != undefined){
            respuesta3 = JSON.parse(respuesta3);
            this.formData.step3 = {
                respuesta:  respuesta3.respuesta,
                dias: respuesta3.dias,
                horasNocturnas: respuesta3.horasNocturnas,
            }
        }
        let respuesta4 = Cookies.get('Respuesta4');
        if(respuesta4 != undefined){
            this.formData.step4 = respuesta4;
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
.submit{ 
    margin-top: 30px;
    padding: 10px 30px;
    color: white;
    background-color: #FF595A;
    font-size: 20px;
    margin-bottom: 10px;
    border-radius: 5px;
    border: none;
    cursor: pointer;
}

.swal2-popup .swal2-styled.confirm {
    padding: 10px 40px;
    color: white;
    background-color: #FF595A;
    font-size: 20px;
    margin-bottom: 10px;
    border-radius: 5px;
    border: none;
    cursor: pointer;
}
.swal2-popup .swal2-styled.cancel{
    padding: 10px 30px;
    color: black;
    background-color: white;
    font-size: 20px;
    margin-bottom: 10px;
    border-radius: 5px;
    border: 1px solid black;
    cursor: pointer;
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
    .submit{
        padding: 10px 30px;
        font-size: 15px;
    }
    .swal2-popup .swal2-styled.containerPopup{
        width: auto;
    }
    
}


</style>