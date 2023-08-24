<script setup>

import {getPressureMm, getTime} from "../utils";

import { computed } from "vue";

const props = defineProps({
    weatherInfo: {
        type: [Object,null],
        required: true
    }
})

const timezone = computed(() => props.weatherInfo?.timezone);

const sunriseTime = computed(() => {
    return getTime(props.weatherInfo?.sys?.sunrise + timezone.value);
})

const sunsetTime = computed(() => {
    return getTime(props.weatherInfo?.sys?.sunset + timezone.value);
})

</script>

<template>
    <div class="section highlights">
        <div class="title">
            Today's Highlights
        </div>
        <div class="highlights-wrapper">
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Wind
                    </div>
                    <div class="card-pic card-pic--wind"></div>
                    <div class="card-info">
                        <div class="card-justify">
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ weatherInfo?.wind?.speed}}
                                </div>
                                <div class="info-main-text">
                                    m/s
                                </div>
                            </div>
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ weatherInfo?.wind?.deg}}
                                </div>
                                <div class="info-main-text">
                                    deg
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Wind gusts
                    </div>
                    <div class="card-small-info">
                        <div v-if="weatherInfo?.wind?.gust" class="card-small-data">
                            <div class="info-main-num">
                                {{ Math.round(weatherInfo?.wind?.gust) }}
                            </div>
                            <div class="info-main-text">
                                m/s
                            </div>
                        </div>
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--wind"></div>
                            <div class="card-small-text text-egorova">
                                Learn
                                <a href="https://www.windy.com/articles/weather-phenomena-what-s-the-difference-between-sustained-winds-and-wind-gusts-10390?satellite,7.787,115.115,5" target="_blank">more</a>
                                about gusts
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Pressure
                    </div>
                    <div class="card-pic card-pic--pressure"></div>
                    <div class="card-info">
                        <div class="card-centered">
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ getPressureMm(weatherInfo?.main?.pressure) }}
                                </div>
                                <div class="info-main-text">
                                    mm
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Feels like
                    </div>
                    <div class="card-small-info">
                        <div class="card-small-data">
                            <div class="info-main-num">
                                {{ Math.round(weatherInfo?.main?.feels_like)}}
                            </div>
                            <div class="info-main-text">
                                °C
                            </div>
                        </div>
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--margin card-small-pic--pressure"></div>
                            <div class="card-small-text">
                                How hot or cold it really feels
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Sunrise and sunset
                    </div>
                    <div class="card-pic card-pic--sun"></div>
                    <div class="card-info">
                        <div class="states">
                            <div class="state">
                                <div class="state-pic"></div>
                                <div class="state-title">
                                    Sunrise
                                </div>
                                <div class="state-time">
                                    {{ sunriseTime }}
                                </div>
                            </div>
                            <div class="state">
                                <div class="state-pic state-pic--flipped"></div>
                                <div class="state-title">
                                    Sunset
                                </div>
                                <div class="state-time">
                                    {{ sunsetTime }}
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Cloudiness
                    </div>
                    <div class="card-small-info">
                        <div class="card-small-data">
                            <div class="info-main-num">
                                {{ weatherInfo?.clouds?.all }}
                            </div>
                            <div class="info-main-text">
                                %
                            </div>
                        </div>
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--sun"></div>
                            <div class="card-small-text">
                                The sky fraction obscured by clouds
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>

@import "../assets/styles/main";

.highlights {
    padding: 1.75rem 1rem 1rem;
    background-color: $card-bcg;
    background-size: cover;
    border-radius: 1.5625rem;
    &-wrapper {
        display: flex;
        justify-content: space-between;
    }
}
.title {
    padding-bottom: 1rem;
}
.highlight {
    width: 32%;
}
.card {
    min-height: 14.375rem;
    padding: 1rem;
    background: -webkit-linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background: -moz-linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background: linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background-size: cover;
    border-radius: 0.5rem;
    &-centered {
        display: flex;
        justify-content: center;
        margin-top: 2.5rem;
    }
    &-justify {
        display: flex;
        justify-content: space-between;
        margin-top: 2.5rem;
    }
    &-title {
        padding-bottom: 0.75rem;
        font-size: 0.8125rem;
    }
    &-pic {
        width: 100%;
        height: 5.625rem;
        margin-bottom: 1rem;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: contain;
        &--wind {
            background-image: url("/src/assets/img/equalizer (2).png");
        }
        &--pressure {
            background-image: url("/src/assets/img/barometer.png");
        }
        &--sun {
            background-image: url("/src/assets/img/sun-moving.png");
        }
    }
}
.states {
    display: flex;
    justify-content: space-between;
    &--margin {
        margin-top: 2.5rem;
    }
}
.state {
    width: 40%;
    &:last-child {
        text-align: right;
    }
    &-pic {
        width: 1.25rem;
        height: 1.25rem;
        margin-bottom: 0.375rem;
        background: url("/src/assets/img/sun.svg") no-repeat 50% 50%;
        background-size: cover;
        &--flipped {
            margin-left: auto;
            -webkit-transform: scaleX(-1);
            transform: scaleX(-1);
        }
    }
    &-title {
        font-size: 0.75rem;
        color: $gold;
    }
    &-time {
        font-size: 0.8125rem;
        font-weight: 700;
    }
}
.info-main {
    display: flex;
    align-items: flex-end;
    &:last-child {
        text-align: right;
    }
    &-num {
        font-size: 1.25rem;
    }
    &-text {
        padding-left: 0.125rem;
        padding-bottom: 0.1875rem;
        font-size: 0.8125rem;
        color: rgba(255, 255, 255, 0.75);
    }
}
.card-small {
    margin-top: 0.75rem;
    padding: 0.75rem 1rem;
    background: -webkit-linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background: -moz-linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background: linear-gradient(0deg, rgb(40, 62, 82), rgb(75, 121, 160) 51%, rgb(40, 62, 82) 100%);
    background-size: cover;
    border-radius: 0.5rem;
    min-height: 6rem;
    &-title {
        font-size: 0.8125rem;
    }
    &-info {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    &-pic {
        width: 1.25rem;
        height: 1.25rem;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: contain;
        &--margin {
            width: 1rem;
            height: 1rem;
            margin-bottom: 0.1875rem;
        }
        &--wind {
            background-image: url("/src/assets/img/gusts.svg");
        }
        &--pressure {
            background-image: url("/src/assets/img/humidity.svg");
        }
        &--sun {
            background-image: url("/src/assets/img/cloud.svg");
        }
    }
    &-data {
        display: flex;
        align-items: flex-end;
        width: 45%;
    }
    &-hint {
        width: 55%;
    }
    &-text {
        font-size: 0.6875rem;
        line-height: 1.2;
        color: rgba(255, 255, 255, 0.6);
    }
}
@media (max-width: 1199px) {
    .card {
        padding: 0.75rem;
        &-title {
            font-size: 0.75rem;
        }
        &-small{
            &-info {
                flex-direction: column;
                align-items: flex-start;
            }
            &-pic {
                display: none;
            }
            &-data {
                width: 100%;
                padding-top: 0.5rem;
            }
            &-hint {
                width: 100%;
            }
            &-text {
                min-height: 1.375rem;
                font-size: 0.5625rem;
            }
        }
    }
    .state-time {
        font-size: 0.6875rem;
    }
    .info-main{
        &-num {
            font-size: 1.125rem;
        }
        &-text {
            padding-bottom: 0.09375rem;
            font-size: 0.75rem;
        }
    }
}
@media (max-width: 575px) {
    .card-small{
      min-height: auto;
    }
    .highlight {
        width: 100%;
        margin-bottom: 1rem;
    }
    .highlights-wrapper {
        flex-direction: column;
    }
}
</style>