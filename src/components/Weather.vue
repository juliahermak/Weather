<template>
  <div class="wrapper">
<h1>Weather </h1>
    <p>Дізнатись погоду y {{city==""? "вашому місті" : cityName}}</p>
    <input type="text" v-model="city" placeholder="Вкажіть місто англійською">
    <button @click="getWeather()" v-show="city != ''">Отримати дані</button>
    <p class="error">{{error}}</p>

    <div  v-if="info != null">
      <p class="nameCity">{{city}}</p>
      <p > {{showTemp}}</p>
      <p>{{showFeelsLike}}</p>
      <p>{{showMinTemp}}</p>
      <p>{{showMaxTemp}}</p>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
data(){
  return{
    city:"",
    error:"",
    info:null,
  }
},
computed:{
  cityName(){
    return "місті " + this.city
  },
  showTemp(){
    return "Температура: " + this.info.main.temp + "°C"
  },
  showFeelsLike(){
    return "Відчувається як: " + this.info.main.feels_like + "°C"
  },
  showMinTemp(){
    return "Мінімальна температура: " + this.info.main.temp_min + "°C"
  },
  showMaxTemp(){
    return "Максимальна температура: " + this.info.main.temp_max + "°C"
  },

},
  methods:{
  getWeather(){
     if(this.city.trim().length<2){
       this.error ="* Вкажіть назву більше одної літери"
       return false
     }
    this.error=""
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=667ad2b3f727fd7f7cd9cf0f0032e155`)
        .then(res => (this.info = res.data))

  }
  }
}
</script>
<style>
.nameCity{
  font-weight: bolder;
  font-size: 20px;
  color: #ffccff;
  padding-bottom: 16px;
  border-bottom: 4px solid darkmagenta;
}

.error{
  color:  cornflowerblue;
  font-weight: bolder;
  font-size: 16px;
;
}
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: darkslateblue;
  text-align: center;
  color: white;
  box-shadow: rgb(85, 91, 255) 0px 0px 0px 3px, rgb(31, 193, 27) 0px 0px 0px 6px, rgb(255, 217, 19) 0px 0px 0px 9px, rgb(255, 156, 85) 0px 0px 0px 12px, rgb(255, 85, 85) 0px 0px 0px 15px;
}
.wrapper h1{
  margin-top:30px
}
.wrapper p{
  margin-top: 20px;
}
.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid darkviolet;
  color: #ffccff;
  padding: 6px 8px;
  font-size: 16px;
  outline: none;
}
.wrapper button{
  background: #ffccff;
  color: darkviolet;
  border-radius: 10px;
  border: 2px solid  plum;
  margin-left: 20px;
  padding: 8px 8px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
.wrapper div{
  border: 4px solid darkmagenta;
  border-radius: 16px;
  padding-bottom: 16px;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  margin-top: 48px;
  width: 300px;
}
</style>