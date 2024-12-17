<template>
    <div class="calendar-container">
        <h2>Calendario</h2>

        <div>
            <FullCalendar 
                :options="calendarOptions"
            />
        </div>
    </div>
</template>

<script setup>
import { format } from 'date-fns';
import { computed, ref } from 'vue';
import FullCalendar from '@fullcalendar/vue3';
import dayGridPlugin from '@fullcalendar/daygrid';
import interactionPlugin from '@fullcalendar/interaction';


const props = defineProps(["subscriptions"]);

const calendarOptions = {
    plugins: [ dayGridPlugin, interactionPlugin ],
    initialView: 'dayGridMonth',
    headerToolbar: {
        left: 'prev, next today',
        center: 'title',
        right: 'dayGridMonth, dayGridWeek',
    },
    events: computed(() => {
        let eventss = [];
        props.subscriptions.forEach((sub) => {
            const renova = calculateRenova(sub.FechaInicio);
            eventss = [...eventss, ...renova];
        })

        return eventss;
    })

};


const calculateRenova = (initialDate, month = 6) => {
    const dates = [];
    let date = new Date(initialDate);

    for (let i = 0; i < month; i++) {
        dates.push({
            title: 'Renovación',
            start: new Date(date).toISOString().split('T')[0],
        });
        date.setMonth(date.getMonth() + 1)
    }

    return dates;
}


</script>

<style lang="scss" scoped>

h2 {
    font-size: 3rem;
    margin: 3rem 0rem;
}


.calendar-container {
    display: flex;
    width: 100%;
    flex-direction: column;
    justify-content: center;
}

</style>