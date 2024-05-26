<script>
import axios from 'axios'
export default {
    data() {
        return {
            city: "",
            error: "",
            info: null
        }
    },
    computed: {
        showTemp() {
            return "Температура: " + this.info.main.temp
        },
        showFeelsLike() {
            return "Ощущается как: " + this.info.main.feels_like
        },
        showMinTemp() {
            return "Минимальная температура: " + this.info.main.temp_min
        },
        showMaxTemp() {
            return "Максимальная температура: " + this.info.main.temp_max
        },
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2) {
                this.error = "Название города не может быть одним символом"
                return false
            }
            
            this.error = ""

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ddb2d692aad33bd33d1be92ad05821d4`)
                .then(res => (this.info = res.data))
        }
    }
}
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == "" ? "Вашем городе" : city }}</p>
        <input type="text" v-model="city" placeholder="Введите город">
        <button v-show="city != ''" @click="getWeather()">Получить погоду</button>
        <p class="error">{{ error }}</p>

        <div v-if="info != null">
            <p>{{ showTemp }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>

<style scoped>
.error {
    color: red;
}
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: rgba(86,105,123, 0.2);
    text-align: center;
    color: rgb(255, 255, 255);
}

.wrapper h1 {
    margin-top: 50px;
}

.wrapper p {
    margin-top: 20px;
}

.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: none;
    border-bottom: 2px solid #030d16;
    color: rgb(255, 255, 255);
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    font-family: "Montserrat", sans-serif;
}

.wrapper input:focus {
    border-bottom-color: #6751bd;
}

.wrapper button {
    background: #123657;
    color: rgb(255, 255, 255);
    border-radius: 10px;
    border: 2px solid #0c243b;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
    font-family: "Montserrat", sans-serif;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
    color: #a099c9;
}
</style>
