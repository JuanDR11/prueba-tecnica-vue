<template>
    <div class="upload">
        <h2>Subir archivo</h2>

        <input type="file" @change="fileUpload" accept=".csv" />
        <p v-if="error">{{ error }}</p>

        <hr>
    </div>



</template>

<script setup>
import Papa from 'papaparse';
import { ref, defineEmits } from 'vue'

const error = ref(null);
const emit = defineEmits(['dataUploaded']);

const fileUpload = () => {
    const file = event.target.files[0];
    if (!file) return;


    Papa.parse(file, {
        header:true,
        skipEmptyLines: true,
        complete: (result) => {
            if (validateCsv(result.data)) {
                emit('dataUploaded', result.data);
            } else {
                error.value = "Formato inválido DEBE SER CSV-- Asegurate de incluir los siguientes campos: Nombre, Tipo de suscripción, Fecha de inicio, Precio";
            }

        },
    });

}


const validateCsv = (data) => {
    return data.every(row => row.Nombre && row.Tipo && row.FechaInicio && row.Precio );
}


</script>

<style lang="scss" scoped>

.upload {
    display: flex;
    flex-direction: column;

    h2 {
        font-size: 3rem;
        margin-bottom: 2rem;
    }

    hr {
        margin: 2rem 0rem;
    }
}


</style>