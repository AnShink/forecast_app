<script setup>

import {ref, onMounted, computed} from "vue";
import {API_KEY, BASE_URL} from "./constants"
import {capitalizeFirstLetter} from './utils/index.js';

import WeatherSummary from "./components/WeatherSummary.vue";
import Highlights from "./components/Highlights.vue";
import Coords from "./components/Coords.vue";
import Humidity from "./components/Humidity.vue";

const city = ref('Omsk')
const weatherInfo = ref(null)
const isError = computed(() => weatherInfo.value?.cod !== 200)

function getWeather(){
    fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => weatherInfo.value = data)
}

onMounted(getWeather)
</script>

<template>
    <div class="page">
        <main class="main">
            <div class="container">
                <div class="laptop">
                  <marquee behavior="alternate" direction="left" scrolldelay="70">
                    WEATHER FORECAST
                  </marquee>
                    <div class="sections">
                        <section :class="['section', 'section-left', { 'section-error': isError }]">
                            <div class="info">
                                <div class="city-inner">
                                    <input
                                        v-model="city"
                                        type="text"
                                        class="search"
                                        @keyup.enter="getWeather"
                                    >
                                </div>
                                <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo"/>
                                <div v-else class="error">
                                    <div class="error-title">
                                        Ooooops! Something went wrong
                                    </div>
                                    <div v-if="weatherInfo?.message" class="error-message">
                                        {{ capitalizeFirstLetter(weatherInfo?.message) }}
                                    </div>
                                </div>
                            </div>
                        </section>
                        <section v-if="!isError" class="section section-right">
                            <Highlights :weatherInfo="weatherInfo"/>
                        </section>
                    </div>
                    <div v-if="!isError" class="sections">
                        <Coords :coord="weatherInfo.coord"/>
                        <Humidity :humidity="weatherInfo.main.humidity"/>
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>

<style scoped lang="scss">

@import "./assets/styles/main";
.page {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 1.25rem 0;
    background: -webkit-linear-gradient(180deg, rgb(75, 121, 160), rgb(40, 62, 82));
    background: -moz-linear-gradient(180deg, rgb(75, 121, 160), rgb(40, 62, 82));
    background: linear-gradient(180deg, rgb(75, 121, 160), rgb(40, 62, 82));
}
.laptop {
    width: 100%;
    padding: 1.25rem;
    //background-color: #0e100f;
    border-radius: 1.5625rem;
}
.sections {
    display: flex;
    width: 100%;
}
.section {
    &-left{
        width: 30%;
        padding-right: 0.625rem;
    }
    &-right {
        width: 70%;
        padding-left: 0.625rem;
    }
    &-error{
        min-width: 14.6875rem;
        width: auto;
        padding-right: 0;
    }
}
.city-inner {
    position: relative;
    display: inline-block;
    width: 100%;
}
.city-inner::after {
    content: '';
    position: absolute;
    top: 0;
    right: 0.625rem;
    width: 1.5625rem;
    height: 1.5625rem;
    background: url("./assets/img/search.svg") no-repeat 50% 50%;
    background-size: contain;
    transform: translateY(50%);
    cursor: pointer;
}
.info {
    height: 100%;
    padding: 1rem;
    background: $card-bcg;
    background-size: cover;
    border-radius: 1.5625rem;
}
.search {
    width: 100%;
    padding: 1rem;
    font-family: 'Inter', Arial, sans-serif;
    color: white;
    background-color: #283e52;
    border-radius: 1rem;
    border: none;
    outline: none;
    cursor: pointer;
}
.section-bottom {
    width: 50%;
    margin-top: 1rem;
}

.error{
    padding-top: 1.25rem;
    &-title{
        font-size: 1.125rem;
        font-weight: 700;
    }
    &-message{
        padding-top: 0.625rem;
        font-size: 0.75rem;
    }
}
@media (max-width: 1199px) {
  .sections{

  }
}
@media (max-width: 767px) {
    .sections {
        flex-direction: column;
    }
    .section{
        &-left {
            width: 100%;
            padding-right: 0;
        }
        &-right {
            width: 100%;
            margin-top: 1rem;
            padding-left: 0;
        }
        &-bottom {
            width: 100%;
        }
    }
}
</style>
