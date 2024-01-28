<template>
    <div class = "agenda-form">
        <form>
            <div class = "input-group">
                <p class = "error" v-if = "errors.title">{{ errors.title }}</p>
                <input v-model = "form.title" placeholder = "Title">
            </div>
            <textarea class = "description" v-model = "form.description" placeholder = "Description"></textarea>
            <div class = "date-grid">
                <p class = "error" v-if = "errors.start_date">{{ errors.start_date }}</p>
                <label>Start Date: </label>
                <input type = "datetime-local" v-model = "form.start_date">
                <p class = "error" v-if = "errors.end_date">{{ errors.end_date }}</p>
                <label>End Date: </label>
                <input type = "datetime-local" v-model = "form.end_date">
            </div>
            <input type = "submit" value = "Save" @click.prevent = "submit">
        </form>
    </div>
</template>

<script>
    import axios from "axios"
    import moment from "moment"
    export default {
        name: "AgendaForm",
        props: {
            agendaDate: {
                type: String,
                required: true
            },
            existingEvent: {
                type: Object,
                required: false
            }
        },
        data() {
            return {
                errors: {},
                form: {
                    title: "",
                    description: "",
                    start_date: null,
                    end_date: null
                }
            }
        },
        methods: {
            validate () {
                this.errors = {}
                if (this.form.title && this.form.start_date && this.form.end_date) {
                    return true
                }
                if (!this.form.title) {
                    this.errors.title = "Event is missing title"
                }
                if (!this.form.start_date) {
                    this.errors.start_date = "Event is missing start date"
                }
                if (!this.form.end_date) {
                    this.errors.end_date = "Event is missing end date"
                }
                return false
            },
            async submit () {
                if (this.validate()) {
                    if (this.existingEvent ) {
                        await this.updateEvent(this.form, this.existingEvent.id)
                    } else {
                        await this.saveEvent(this.form)
                    }
                    this.$emit("close")
                    this.$emit("submitted")
                }      
            },
            async saveEvent(eventObj) {
                return axios.post("http://localhost:4000/submitEvent", { title: eventObj.title, description: eventObj.description, start_date: eventObj.start_date, end_date: eventObj.end_date})
                    .then(response => {
                    return response;
                })
                    .catch((error) => {
                    console.log(error);
                });
            },
            async updateEvent(eventObj, eventId) {
                return axios.post("http://localhost:4000/updateEvent", { id: eventId, title: eventObj.title, description: eventObj.description, start_date: eventObj.start_date, end_date: eventObj.end_date})
                    .then(response => {
                    return response;
                })
                    .catch((error) => {
                    console.log(error);
                });
            }
        },
        mounted () {
            if (this.existingEvent) {
                this.form.title = this.existingEvent.title;
                this.form.description = this.existingEvent.description;
                this.form.start_date = moment(this.existingEvent.start_date).format('YYYY-MM-DDThh:mm');
                this.form.end_date = moment(this.existingEvent.end_date).format('YYYY-MM-DDThh:mm');
            } else if (this.agendaDate) {
                const targetDate = new Date(this.agendaDate);
                const targetValue = (new Date(targetDate.getTime()).toISOString()).slice(0, -1);
                this.form.start_date = targetValue;
            }
        }
    }
</script>

<style lang = "scss">
    .agenda-form {
        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            .error {
                margin: 0;
                font-size: 12px;
                color: red;
                font-family: 'Roboto', sans-serif;
                grid-column: 1/-1;
            }
            input {
                font-family: 'Roboto', sans-serif;
            }
            .description {
                resize: none;
                font-family: 'Roboto', sans-serif;
            }
            .date-grid {
                display: grid;
                grid-template-columns: max-content 1fr;
                gap: 10px;
                align-items: center;
                label {
                    font-family: 'Roboto', sans-serif;
                    font-size: 14px;
                }
                .error {
                    margin-bottom: -5px;
                }
            }
        }
    }
</style>