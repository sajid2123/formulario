<template>
    <div class="form-input">
        <h1 class="title-step5">¿A quién se lo enviamos?</h1>
        <h2 class="subtitle-step5">Te llegará inmediatamente</h2>

        <div class="field" style="max-width: 400px;">
            <label class="label">Nombre</label>
            <input type="text" class="input" name="customer[name]" required>
        </div>
        <div class="field" style="max-width: 400px;">
            <label class="label">Correo electrónico</label>
            <input type="email" class="input"  name="customer[email]" required>
        </div>
        <div class="field" style="max-width: 400px;">
            <label class="label">Teléfono</label>
            <input type="number" class="input"  name="customer[phone]" required>
        </div>
        <div class="field" style="max-width: 400px;">
            <label class="label">Código postal</label>
            <input type="number" class="input"  name="customer[cp]" required>
        </div>
        <div class="notification">No te preocupes, no compartimos tus datos y los usaremos solamente para elaborar este presupuesto.</div>
        <div class="field" style="max-width: 400px;">
            <label class="checkbox">
                <input type="checkbox"  name="customer[terms]" required>
                Acepto los <a href="/condiciones-de-uso" target="_blank">Términos y Condiciones</a> y la <a href="/privacidad" target="_blank">Política de privacidad</a>.
            </label>
        </div>
        <div class="proteccion-de-datos">
            <p> <span class="underline">Protección de datos</span>  El responsable del tratamiento es AUXISTENCIA S.L.
              La finalidad de la recogida de sus datos es para poder atender sus solicitudes de información,
              sin ceder sus datos a terceros. Tiene derecho a saber qué información tenemos sobre usted,
              corregirla o eliminarla tal y como se explica en la Política de Privacidad.</p>
        </div>
    </div>
</template>
  
<script>
  export default {
    name: 'Step1',
}
</script>

<style scoped>
h1{
  font-size: 40px;
}
.title-step5{
  color: #0C2E69;
}
.subtitle-step5{
  color: #8596B4;
}
.field{
  display: flex;
  flex-direction: column;
  margin-top: 10px;
}
.notification{
  background-color: #F5F5F5;
  color: #002E6D;
  width: 400px;
  padding: 20px;
  margin-top: 10px;
  border-radius: 5px;
}
a{
  color: #FF595A;
  text-decoration: none;
}
.checkbox{
  margin-top: 15px;
}
label{
  font-size: 18px;
  color: #002E84;
}
.input{
  padding: 6px 15px;
  font-size: 18px;
  border-radius: 3px;
  border: 1px solid #666666;
}
.proteccion-de-datos{
  margin-top: 10px;
  font-size: 10px;
  width: 400px;
  color: #002E6D;
}
.underline{
  text-decoration: underline;
}
@media screen and (max-width: 450px) {
  .proteccion-de-datos{
    width: auto;
  }
  .notification{
    width: auto;
  }
  h1{
    font-size: 30px;
  }
}
</style>