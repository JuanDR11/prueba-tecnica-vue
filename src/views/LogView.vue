<template>
    <div>
        <h1>Cambio de base</h1>
        <div>
            <label for="ammount">Ingresa el número X (base 10)</label>
            <input 
                type="number"
                id="change"
                name="ammount"
                v-model="ammount"
                placeholder="Ingresa el número x">
        </div>

        <div>
            <label for="base">Ingresa la base (K)</label>
            <input 
                type="number"
                id="base"
                name="base"
                v-model="base"
                placeholder="Ingresa el número k">
        </div>


        <button  @click="changeBase()">Calcular</button>

        <div>
            <h2>Resultado :</h2>
            <span> número: <strong>{{ result.join('') }}</strong></span> <span>Base: {{ base }}</span>
        </div>

        <div>
            <h2>Historial</h2>
            <table border="1">
                <thead>
                    <tr>
                        <th>Paso</th>
                        <th>Cociente</th>
                        <th>Residuo</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in history" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ item.quotient }}</td>
                        <td>{{ item.residue }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

</template>

<script setup>

import { ref } from 'vue';

const ammount = ref(0);
const base = ref(2);
const history = ref([]);
const result = ref([]);



const changeBase = () => {


    if(base.value <= 1) {
        alert("La base debe ser mayour o igual a 2.");
        return;
    }

    let tempQuotient = ammount.value;
    let tempResidue;
    history.value = [];

    // Calculate
    while (tempQuotient > 0) {

        tempResidue = tempQuotient % base.value;
        tempQuotient  =  Math.floor(tempQuotient/base.value);
        result.value.unshift(tempResidue);


        history.value.push({
            ammount: ammount.value,
            base: ammount.value,
            quotient: tempQuotient,
            residue: tempResidue
        })


    }
}



</script>

<style>

</style>