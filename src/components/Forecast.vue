<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section','section-left',{'section-error': isError}]">
              <div class="info">
                <div class="city-inner">
                  <input v-model="city" type="text" class="search" @keyup.enter="getWeather">
                </div>
                <FSummary v-if="!isError" :weatherInfo="weatherInfo"/>
                <div v-else class="error">
                  <div class="error-title">
                    Oooops! Something went wrong
                  </div>
                  <div v-if="weatherInfo?.message" class="error-message">
                    {{capitalizeFirstLetter(weatherInfo?.message)}}
                  </div>
                </div>
              </div>
            </section>
            <section v-if="!isError" class="section section-right">

                <FHighlights  :weatherInfo="weatherInfo"/>

            </section>
          </div>
          <div v-if="!isError" class="sections">
             <FCoords :coord="weatherInfo.coord"></FCoords>
            <FHumidity :humidity="weatherInfo.main.humidity"></FHumidity>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
<script setup>
import {capitalizeFirstLetter} from "../utils"
import  {ref , onMounted , computed} from "vue"
import {API_KEY, BASE_URL} from "../constants"
import FSummary from "./FSummary.vue";
import FCoords from "./FCoords.vue";
import FHumidity from "./FHumidity.vue";
import FHighlights from "./FHighlights.vue";

const city =ref('Paris')
const weatherInfo=ref(null)
const isError = computed(()=> weatherInfo.value?.cod !== 200)

function getWeather(){
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
      .then((res)=>res.json())
      .then((data)=> weatherInfo.value = data)
}
onMounted(getWeather)
</script>
<style lang="sass" scoped>
@import "../assets/styles/_main.sass"
.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #59585d

.laptop
  width: 100%
  padding: 20px
  background-color: #0e100f
  border-radius: 25px

.sections
  display: flex
  width: 100%

  @media (max-width: 767px)
    flex-direction: column

.section-left
  width: 30%
  padding-right: 10px

  @media (max-width: 767px)
    width: 100%
    padding-right: 0

  &.section-error
      min-width: 235px
      width: auto
      padding-right: 0

.section-right
  width: 70%
  padding-left: 10px

  @media (max-width: 767px)
    width: 100%
    margin-top: 16px
    padding-left: 0

.city-inner
  position: relative
  display: inline-block
  width: 100%

  &::after
    content: ''
    position: absolute
    top: 0
    right: 10px
    width: 25px
    height: 25px
    background: url('/src/assets/img/search.svg') no-repeat 50% 50%
    background-size: contain
    transform: translateY(50%)
    cursor: pointer

.info
  height: 100%
  padding: 16px
  background: url('/src/assets/img/gradient-1.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 25px

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgba(0, 0, 0, 0.75)
  border-radius: 16px
  border: none
  outline: none
  cursor: pointer

.section-bottom
  width: 50%
  margin-top: 16px

  @media (max-width: 767px)
    width: 100%

.error
  padding-top: 20px
  &.title
    font-size: 18px
    font-weight: 700
  &.message
   padding-top: 10px
   font-size: 13px

</style>