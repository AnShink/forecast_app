<script setup>

import {capitalizeFirstLetter} from '../utils/index.js'

const props = defineProps({
    weatherInfo: {
        type: [Object,null],
        required: true
    }
})

const today = new Date().toLocaleString('en-EN', { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' })
</script>

<template>
    <div class="summary">
    <div
        :style="`background-image: url('/weather-main/${weatherInfo?.weather[0].description}.png');`"
        class="pic-main"
    ></div>
    <div class="weather">
        <div class="temp">
            {{ Math.round(weatherInfo?.main?.temp) }} °C
        </div>
        <div class="weather-desc text-block">
            {{ capitalizeFirstLetter(weatherInfo?.weather[0].description) }}
        </div>
    </div>
    <div class="city text-block">
        {{ weatherInfo?.name }},
        {{ weatherInfo?.sys?.country }}
    </div>
    <div class="date text-block">
        {{ today }}
    </div>
</div>
</template>

<style lang="scss" scoped>
@import "../assets/styles/main";

.pic-main {
    width: 3.75rem;
    height: 3.75rem;
    margin: 1.25rem 0 0.75rem;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;
}
.city {
    font-size: 1.5rem;
}
.weather {
    margin: 0 0 1.25rem;
    padding: 1.25rem 0;
    border-bottom: 0.0625rem solid rgba(255, 255, 255, 0.4);
}
.temp {
    padding-bottom: 0.5rem;
    font-size: 2rem;
}
.text-block {
    position: relative;
    padding-left: 1.5rem;
    padding-bottom: 0.5rem;
    font-size: 0.875rem;
    &::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 1.25rem;
        height: 1.25rem;
        margin-right: 0.375rem;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: contain;
    }
}
.weather-desc::before {
    background-image: url("/src/assets/img/weather.svg");
}
.city::before {
    background-image: url("/src/assets/img/location.svg");
}
.date::before {
    left: 0.125rem;
    width: 0.9375rem;
    height: 0.9375rem;
    background-image: url("/src/assets/img/calendar.svg");
}
</style>