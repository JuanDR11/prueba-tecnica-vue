<template>
  <div>

    <h2>Visualización de datos</h2>

    <div class="suscripction__searchtype">
        <label for="type">Fitral por Tipo de suscripción:</label>
        <input 
            v-model="filter"
            id="type"
            placeholder="Filtrar por tipo de suscripción"
            type="text">
    </div>
    
    <table>
        <thead>
            <tr>
                <th 
                    v-for="nameColumn in columns"
                    :key="nameColumn" 
                    @click="sortTableColumn(nameColumn)" >
                        {{ nameColumn }} -- <span>{{ getSort(nameColumn) }}</span>
                </th>
            </tr>
        </thead>

        <tbody>
            <tr v-for="subscription in filterData" :key="subscription.Nombre" >
                <td>{{ subscription.Nombre }}</td>
                <td>{{ subscription.Tipo }}</td>
                <td>{{ subscription.FechaInicio }}</td>
                <td>{{ subscription.Precio }}</td>
            </tr>
        </tbody>
    </table>

    <div>
        <p>Total Mensual: <span>{{ totalMoneyMountly }}</span></p>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';

const props = defineProps(["subscriptions"]);
const filter = ref('');
const sortColumn = ref('');
const sortDi = ref('');

const result = ref(0);
const total = ref(0);

const columns = [
    'Nombre',
    'Tipo de suscripción',
    'Fecha de inicio',
    'Precio'
]


const sortTableColumn = (nameColumn) => {
    if (sortColumn.value === nameColumn) {
        return sortDi.value = sortDi.value === 'asc' ? 'desc': 'asc';
    } else {
        sortColumn.value = nameColumn;
        sortDi.value = 'asc';
    }
};

const sortData = computed(() => {
    return [...props.subscriptions].sort((a,b) => {
        const direction = sortDi.value === 'asc' ? 1: -1;

        return a[sortColumn.value] > b[sortColumn.value] ? direction : -direction;
    })
});


const filterData = computed(() => {
    return sortData.value.filter(sub =>
        sub.Tipo.toLowerCase().includes(filter.value.toLocaleLowerCase())
    );
});

const getSort = (column) => {
    if (sortColumn.value !== column) return '';

    return sortDi.value === 'asc' ? 'ASC': 'DESC';
}


const totalMoneyMountly = computed(() => {

   return filterData.value.reduce((sum, sub) => {

        const precio = parseFloat(sub.Precio);

        if (isNaN(sub.Precio)) {
            alert("Precio invalido: NaN");
            return sum;
        }

        if(sub.Tipo === 'Anual') {
            return sum + (precio/3);
        }

        else if (sub.Tipo === 'Trimestral') {
            return sum + (precio/3); 
        } 

        else if (sub.Tipo === 'Semestral') {
            return sum + (precio/6); 
        } 
        
        else {
            return sum + precio;
        }
   },0);

});


</script>

<style lang="scss" scoped>

h2 {
    font-size: 3rem;
}

label {
    font-size: 1.5rem;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin: 1rem 0;
    font-size: 1.5rem;
    text-align: center;
    border: 0.1rem solid #ddd ;
}

th {
    padding: 1rem;
    border: 0.1rem solid #ddd;
    text-transform: uppercase;
    cursor: pointer;

    &:hover {
        transform: scale(1.05);
        color: #fff;
    }
}

td {
    padding: 1rem;
    border: 0.1rem solid #ddd;
}


input {
    margin-left: 1rem;
    font-size: 1rem;
    width: 50%;
    height: 2rem;
    border-radius: 0.5rem;
}


.suscripction {

    &__searchtype {
        margin: 2rem 0rem;
    }

}

p {
    margin-top: 2rem;
    font-size: 2rem;
}

</style>