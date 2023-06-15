<script>
import axios from 'axios'

export default {
  data() {
    return{
      city: "", 
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "<" + this.city + ">"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2){
        this.error = "Нужно название больше одного символа"
        return false 
      }

      this.error = ""     

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=cfb85161e05c0be3ac29870b47935b3b`)
      .then(res => (this.info = res.data))
    }
  }

}

</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
     <button disabled v-else>Введите название города</button>
     <p class="error">{{ error }}</p>


     <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
     </div>
   
  </div>
</template> 

<style scoped>

.wrapper {
  width:900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p{
  margin-top: 50px;
}

.wrapper input{
  margin-top:30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d
}

.wrapper button:disabled{
  background: orange;
  cursor: not-allowed;
}

.wrapper button{
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error{
  color: red;

}

</style>
