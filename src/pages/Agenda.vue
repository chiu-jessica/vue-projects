<template>
    <div id = "agenda-page">
        <div class = "agenda-subnav">
            <span class="material-symbols-outlined" @click = "prevMonth">arrow_circle_left</span>
            {{ monthArray[currentMonthIndex] }} {{ currentYear }}
            <span class="material-symbols-outlined" @click = "nextMonth">arrow_circle_right</span>
        </div>
        <div class = "calendar-container">
            <div class = "calendar">
                <div class = "void-box" v-for="startGap in firstDayIndex" :key="startGap"></div>
                <div class = "date-box" v-for = "(day, index) in dateArray" :key = "index" @click = "() => triggerEventForm(index+1)">
                    <div class = "date-container">
                        <span class = "circle" v-if="index+1 == currentDate"></span>
                        <span class = "date">{{ index+1 }}</span>
                    </div>
                    <div v-for = "event in day" :key = "event.id" class = "event" @click.prevent = "() => updateEventForm(event)">
                        {{ event.title }}
                    </div>
                </div>
                <div class = "void-box" v-for="endGap in remainderBoxes" :key="endGap"></div>
            </div>
        </div>
        <Modal v-if = "targetDate && modalOpen" @close = "closeModal">
            <AgendaForm :agenda-date="targetDate" :existing-event = "existingEvent" @close = "() => modalOpen = false" @submitted = "reloadEvent"></AgendaForm>
        </Modal>
    </div>
</template>

<script>
    import AgendaForm from "@/components/AgendaForm.vue";
    import Modal from "@/components/Modal.vue";
    import axios from "axios"

    export default {
    name: "AgendaPage",
    data() {
        return {
            today: null,
            targetDate: null,
            dateArray: [],
            eventArray: [],
            modalOpen: false,
            existingEvent: null,
            monthArray: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
            currentMonthIndex: 0,
            currentYear: 0
        };
    },
    async mounted() {
        this.today = new Date();
        this.currentMonthIndex = this.today.getMonth();
        this.currentYear = this.today.getFullYear();
        // this.saveEvent();
        this.dateArray = this.initializeDateArray();
        await this.getMonthEvents();
        this.fillDateArray()
    },
    computed: {
        firstDayIndex() {
            // const monthIndex = new Date().getMonth();
            // // const year = new Date().getFullYear();
            return new Date(this.currentYear, this.currentMonthIndex, 1).getDay();
        },
        daysInMonth() {
            const month = this.currentMonthIndex + 1;
            // const year = new Date().getFullYear();
            return new Date(this.currentYear, month, 0).getDate();
        },
        remainderBoxes() {
            return 7 - ((this.daysInMonth + this.firstDayIndex) % 7);
        },
        currentDate() {
            const myDate = new Date();
            if (myDate.getMonth() == this.currentMonthIndex && myDate.getFullYear() == this.currentYear) {
                return new Date().getDate();
            }
            return null;
        }
    },
    methods: {
        saveEvent() {
            axios.post("http://localhost:4000/submitEvent", { title: "front end test", description: "front end test description", start_date: new Date(), end_date: new Date(new Date().getDate() + 1) })
                .then(response => {
                console.log(response);
            })
                .catch((error) => {
                console.log(error);
            });
        },
        triggerEventForm(date) {
            const targetDate = new Date();
            targetDate.setDate(date);
            targetDate.setHours(0);
            targetDate.setMinutes(0);
            targetDate.setSeconds(0);
            targetDate.setMilliseconds(0);
            console.log(targetDate);
            this.targetDate = targetDate;
            this.modalOpen = true;
        },
        updateEventForm(event) {
            this.existingEvent = event;
            this.targetDate = event.start_date;
        },
        async getMonthEvents(monthIndex = null, year = null) {
            await axios.get("http://localhost:4000/getMonthEvents", {params: {month: (monthIndex)?monthIndex+1:monthIndex, year}}).then(
                data => {
                    this.eventArray = data.data.sort((a, b) => new Date(a.start_date) - new Date(b.start_date))
                }
            )
        },
        initializeDateArray() {
            let result = []
            for (let i = 0; i < this.daysInMonth; i++) {
                result[i] = []
            }
            return result
        },
        fillDateArray() {
            for (let i = 0; i < this.eventArray.length; i++) {
                let eventDate = this.convertTimeZone(this.eventArray[i].start_date, "America/Los_Angeles").getDate()
                let endDate = this.convertTimeZone(this.eventArray[i].end_date, "America/Los_Angeles").getDate()
                for (let j = eventDate-1; j < endDate; j++) {
                    this.dateArray[j].push(this.eventArray[i])
                }
            }
        },
        convertTimeZone(date, timezone) {
            return new Date(new Date(date).toLocaleString("en-US", {timeZone:timezone}));
        },
        closeModal() {
            this.modalOpen = false;
            this.existingEvent = null;
            this.targetDate = null;
        },
        async reloadEvent(monthIndex = null, year = null) {
            this.dateArray = this.initializeDateArray();
            await this.getMonthEvents(monthIndex, year);
            this.fillDateArray()
        },
        nextMonth() {
            if (this.currentMonthIndex >=11) {
                this.currentMonthIndex = 0;
                this.currentYear++;
            } else {
                this.currentMonthIndex++;
            }
            this.reloadEvent(this.currentMonthIndex, this.currentYear);
        },
        prevMonth () {
            if (this.currentMonthIndex <=0) {
                this.currentMonthIndex = 11;
                this.currentYear--;
            } else {
                this.currentMonthIndex--;
            }
            this.reloadEvent(this.currentMonthIndex, this.currentYear);
        }
    },
    components: { Modal, AgendaForm }
}
</script>

<style lang = "scss">
    .agenda-subnav {
        padding:15px;
        display: flex;
        justify-content: space-between;
        font-family: 'Merriweather', serif;
        font-size: 25px;
    }
    .calendar-container {
        height: 100vh;
        .calendar {
            display: grid;
            width: 100%;
            height: 100%;
            grid-template-columns: repeat(7, 1fr); 
            border-bottom: 1px solid black;
            grid-auto-rows: 1fr;
            .void-box {
                background-color: rgb(198, 202, 207);
            }
            .date-box, .void-box {
                border-top: 1px solid black;
                border-left: 1px solid black;
                position: relative;
                padding: 5px 7px;
                // &:nth-of-type(1) {
                //     grid-column-start: 6;
                // }
                .date-container {
                    position: relative;
                    width: min-content;
                    .circle {
                        position: absolute;
                        top: 50%;
                        left: 50%;
                        transform: translate(-50%, -50%);
                        height: 25px;
                        width: 25px;
                        background-color: rgba(117, 211, 255, 0.616);
                        border-radius: 50%;
                        z-index: 1;
                        display: flex;
                        align-items: center;
                        justify-content: center;
                    }
                    .date {
                        position: relative;
                        z-index: 2;
                    }
                }
            }
            .event {
                background-color: rgb(159, 212, 255);
                width: auto;
                margin: 5px 0px;
                padding: 4px 0 4px 4px;

            }
        }
    }
</style>