<template>
    <div id = "weather">
        <div v-if = "!isEmpty(weather)" id = "app-container" :class = "{night:dayStatus=='night'}">
            <div id = "top">
                <div id = "reload" :class = "{active: isReloading}" @click = "reload">
                    <span class="material-symbols-outlined">refresh</span>
                </div>
                <h1 id = "city-name">Aliso Viejo</h1>
                <div id = "weather-img">
                    <img :src = "weatherImg"/>
                </div>
                <h1 id = "temp">{{ temperature }}&#8457;</h1>
            </div>
            <div id = "bottom">
                <div id = "temp-grid">
                    <div id = "feels-like">
                        <p>Feels like:</p>
                        {{ Math.round((weather.data.feels_like*1.8)+32) }}&#8457;
                    </div>
                    <div id = "low-temp">
                        <p>Low:</p>
                        {{ Math.round((weather.data.min_temp*1.8)+32) }}&#8457;
                    </div>
                    <div id = "high-temp">
                        <p>High:</p>
                        {{ Math.round((weather.data.max_temp*1.8)+32) }}&#8457;
                    </div>
                </div>
                <div class = "other-grid">
                    <div id = "humidity">
                        <p>Humidity:</p>
                        {{ weather.data.humidity }}%
                    </div>
                    <div id = "wind">
                        <p>{{ windStatus }}</p>
                        {{ Math.floor(weather.data.wind_speed) }} mph
                    </div>
                </div>
                <div class = "other-grid">
                    <div id = "sunrise">
                        <p>Sunrise:</p>
                        {{ sunriseString }}
                    </div>
                    <div id = "sunset">
                        <p>Sunset:</p>
                        {{ sunsetString }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: "WeatherPage",
    data () {
        return {
            weather: {},
            isReloading: false
        }
    },
    mounted() {
        axios.get("http://localhost:4000/getWeather", {})
        .then(response => {
            this.weather = response
            console.log(response)
        })
        .catch((error) => {
            console.log(error)
        })
    },
    computed: {
        temperature() {
            if (!this.isEmpty(this.weather)) {
                const celsius = this.weather.data.temp
                return Math.round((celsius*1.8)+32)
            }
            return 0;
        },
        windStatus() {
            if (!this.isEmpty(this.weather)) {
                const wind = Math.floor(this.weather.data.wind_speed)
                if (wind==0) {
                    return "Calm"
                } else if (wind<10) {
                    return "Light breeze"
                } else if (wind<30) {
                    return "Breezy"
                } else {
                    return "Windy"
                }
            }
            return 0;
        },
        dayStatus() {
            if (this.isEmpty(this.weather)) {
                return 0;
            }
            const sunrise = new Date (this.weather.data.sunrise*1000)
            const sunset = new Date (this.weather.data.sunset*1000)
            const currentTime = new Date ()
            if (currentTime.getHours()<sunrise.getHours() || currentTime.getHours()>sunset.getHours()) {
                return "night"
            } else if (currentTime.getHours()>sunrise.getHours() && currentTime.getHours()<sunset.getHours()) {
                return "day"
            } else {
                if (currentTime.getHours()==sunrise.getHours()) {
                    if (currentTime.getMinutes()>sunrise.getMinutes()) {
                        return "day"
                    } else {
                        return "night"
                    }
                } else {
                    if (currentTime.getMinutes()<sunset.getMinutes()) {
                        return "day"
                    } else {
                        return "night"
                    }
                }
            }
        },
        weatherImg() {
            if (this.isEmpty(this.weather)) {
                return 0;
            }
            if (this.dayStatus == "day") {
                return "/weather_icons/1530392_weather_sun_sunny_temperature_icon.png"
            } else {
                return "/weather_icons/1530382_weather_moon_moonlight_night_icon.png"
            }
        },
//make sunrise and sunset so that if minutes is less than 10 it will add a 0 to the front of it
        sunriseString() {
            if (this.isEmpty(this.weather)) {
                return 0;
            }
            const sunrise = new Date (this.weather.data.sunrise*1000)
            return sunrise.getHours() + ":" + this.formatMinutes(sunrise.getMinutes()) + " A.M."
        },
        sunsetString() {
            if (this.isEmpty(this.weather)) {
                return 0;
            }
            const sunset = new Date (this.weather.data.sunset*1000)
            return (sunset.getHours()-12) + ":" + this.formatMinutes(sunset.getMinutes()) + " P.M."
        }
    },
    methods: {
        isEmpty (obj) {
            return Object.keys(obj).length == 0
        },
        formatMinutes (minutes) {
            if (minutes < 10) {
                return "0" + minutes;
            }
            return minutes;
        },
        async reload() {
            this.isReloading = true
            await axios.get("http://localhost:4000/getNewWeather", {})
            .then(response => {
                console.log(response)
                axios.get("http://localhost:4000/getWeather", {})
                .then(response => {
                    this.weather = response
                    console.log(response)
                })
                .catch((error) => {
                    console.log(error)
                });
            })
            .catch((error) => {
                console.log(error)
            });
            setTimeout(() => {
                this.isReloading = false
            }, 1000)
        }
    }
}
</script>

<style lang = "scss">
    #app-container {
        height: 500px;
        width: 450px;
        background-color: rgb(92, 188, 243);
        margin: 50px auto;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 15px;
        &.night {
            background-color: rgb(0, 0, 73);
            color: white !important;
        }
        #top {
            position: relative;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            #reload {
                position: absolute;
                top: 10px;
                right: 15px;
                cursor: pointer;
                &.active {
                    -webkit-animation-name: spin;
                    -webkit-animation-duration: 1s;
                    -webkit-animation-iteration-count: 1;
                    -webkit-animation-timing-function: ease;
                }
            }
            h1 {
            text-align: center;
            margin: 0;
            font-family: 'Playfair Display', serif;
            }
            #weather-img {
                height: auto;
                img {
                    height: 200px;
                    display: block;
                }
            }
        }
        #bottom {
            width: 100%;
            #temp-grid {
                display: grid;
                width: auto;
                grid-template-columns: 1fr 1fr 1fr;
                padding: 0 40px;
                div {
                    display: flex;
                    flex-direction: column;
                    align-items: center;
                    p {
                        margin-bottom: 10px;
                    }
                }
            }
            .other-grid {
                display: grid;
                width: auto;
                grid-template-columns: 1fr 1fr;
                padding: 0 100px;
                div {
                    display: flex;
                    flex-direction: column;
                    align-items: center;
                    p {
                        margin-bottom: 10px;
                    }
                }
            }
        }
        @-webkit-keyframes spin {
            from {
                -webkit-transform: rotate(0deg);
            }
            to {
                -webkit-transform: rotate(360deg);
            }
        }
    }

</style>