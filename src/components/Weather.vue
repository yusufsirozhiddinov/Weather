<template>
    <div class="content">
      <h1 class="text-center pt-5">Прогноз погоды</h1>
      <div class="weather-icons text-center">
           <!-- <div class="snow">
            <div class="snow-block">
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
          </div>
          </div> -->
        <!-- <div class="sunny"></div> -->
      </div>    
    <div class="row">
      <!-- @click="weatherApi()" -->
      <input type="text" class="form-control mt-4" placeholder="Введите страну">
      <button  class="btn-search mt-4">Найти</button>
      <div class="info">
        <h3 class="mt-3 text-center" v-for="(y, r) in arr" :key="r">{{y.name}}</h3>
        <h4 class="text-center"  v-for="(y, r) in arr" :key="r">{{y.main}}</h4>
        <h2 class="temp text-center" v-for="(y, r) in arr" :key="r">{{y.temp}}</h2>
      </div>
    </div>
    </div>
</template>
<script>
export default {
  props: ['reports'],
  name: 'App',
  data: () => ({
    arr : [

    ]
  }),
  
  methods: {
    weatherApi() {
      let input = document.querySelector('input')
      if (input.value.length == 0) {
        input.style.border = '1px solid red'
      } else {
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${input.value}&appid=1cd30991dd9bd85fa384b9abc5096a2b&units=metric`)
        .then(res => res.json())
        .then(json => {
          console.log(json);
          let weatherIc = document.querySelector('.weather-icons')
          switch (json['weather'][0]['main']) {
            case 'Clouds':
              weatherIc.innerHTML = '<div class="x2"><div class="cloud"></div></div>'
              break;
            case 'Clear':
              weatherIc.innerHTML = '<div class="sunny"></div>'  
              break;
            case 'Snow':
              weatherIc.innerHTML = ` <div class="snow">
              <div class="snow-block">
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
          </div>
          </div>`
              break;
            case 'Rain':
              weatherIc.innerHTML = ` <div class="rain">
            <div class="rain-block">
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
          </div>
          </div>`
          }
          let h3 = document.querySelector('h3')
          let h4 = document.querySelector('h4')
          let h2 = document.querySelector('h2')
          h3.innerHTML = json['name']
          h4.innerHTML = json['main']['temp']
          h2.innerHTML = json['weather'][0]['main']
          }) 
      input.style.border = '1px solid gray'
      }
      
} 
},
mounted: function() {
    console.log("getLocation Called");
    var cityApi = 'https://api.bigdatacloud.net/data/reverse-geocode-client' 
      navigator.geolocation.getCurrentPosition(
      (position) => {
      cityApi =
      cityApi +
      "?latitude=" +
      position.coords.latitude +
      "&longitude=" +
      position.coords.longitude +
      "&localityLanguage=en"
      fetch(cityApi)
      .then(res => res.json())
      .then(json => {
      let cityn = json['city']
      let z = {
        city: cityn,
      }
      this.arr.unshift(z)
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${cityn}&appid=1cd30991dd9bd85fa384b9abc5096a2b&units=metric`)
        .then(res => res.json())
        .then(json => {
          console.log(json);
          
          let weatherIc = document.querySelector('.weather-icons')
          switch (json['weather'][0]['main']) {
            case 'Clouds':
              weatherIc.innerHTML = '<div class="x2"><div class="cloud"></div></div>'
            case 'Clear':
              weatherIc.innerHTML == '<div class="sunny"></div>'
              break;
            case 'Snow':
              weatherIc.innerHTML = ` <div class="snow">
              <div class="snow-block">
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
              <article class="snow-"></article>
          </div>
          </div>`
              
              break;
            case 'Rain':
              weatherIc.innerHTML = ` <div class="rain">
            <div class="rain-block">
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
              <article class="rain-"></article>
          </div>
          </div>`
            }
          let weatherInfo = {
            name: json['name'],
            temp: json['main']['temp'],
            main: json['weather'][0]['main'],
            tempmax: json['main']['temp_max'],
            tempmin: json['main']['temp_min'],
            feelslike: json['main']['feels_like']
          }
          this.arr.unshift(weatherInfo)
          
        })
        
      })
    },)
},
}
</script>

<style scopped>
@import url('https://fonts.googleapis.com/css2?family=KoHo:wght@500&display=swap');
body{
  font-family: 'KoHo', sans-serif !important;
}
.row{
  display: flex !important;
  justify-content: space-between;
  margin-top: 100px;
}
.btn-search{
  width: 200px !important;
  background-color: #fff;
  color: #000;
  border: 1px solid #000;
  border-radius: 5px;
  transition: .5s;
}

.btn-search:hover{
  background-color: rgb(56, 22, 59);
  border-radius: 50px;
  color: #fff;
}

.form-control{
  width: 1100px !important;
}

img{
  width: 100px;
  height: 100px;
}
.weather-icons{
  display: flex;
  justify-self: center;
}
.temp{
  font-size: 50px !important;
  font-weight: bold;
}
@media (max-width: 4000px) {

  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-right: -242px !important;
}
.snow{
  margin-left: 470px;
}
.rain{
  margin-left: 470px;
}
}
@media (max-width: 1400px) {
  .container{
    width: 1200px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
    height: 40px;
  }
  input{
    width: 100% !important;
  }
    .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-right: -242px !important;

}
.snow{
  display: flex;
  justify-self: center;
  margin-left: 300px;
}
.rain{
  margin-left: 470px;
}
}
@media (max-width: 1000px) {
  .container{
    width: 950px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
    .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-right: -150px !important;
  display: flex;
}
}
@media (max-width: 900px) {
  .container{
    width: 850px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
    .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-right: -150px !important;

}

}
@media (max-width: 800px) {
  .container{
    width: 750px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 0px !important;
}
.snow{
  margin-left: 470px;
}
.rain{
  margin-left: 470px;
}
.sunny{
  margin-left: 300px !important;
}
}
@media (max-width: 700px) {
  .container{
    width: 650px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 0px !important;
}
.rain{
  margin-left: 470px;
}
.sunny{
  margin-left: 200px !important;
}
}
@media (max-width: 600px) {
  .container{
    width: 500px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 0px !important;
}
.snow{
  margin-left: 470px;
}
.rain{
  margin-left: 470px;
}
.sunny{
  margin-left: 0 !important;
}
}
@media (max-width: 500px) {
  .container{
    width: 300px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 0px !important;
}
.weather-icons{
  display: flex;
  justify-content: center;
}
.snow{
  margin-left: 0  !important;
}
.rain{
  margin-left: 470px;
}
.sunny{
  margin-left: 0 !important;
}
}
@media (max-width: 400px) {
  .container{
    width: 300px !important;
  }
  .btn-search{
    margin-top: 30px !important;
    width: 100% !important;
  }
  .x2 {
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 0px !important;
}
.weather-icons{
  display: flex;
  justify-content: center;
}
.snow{
  margin-left: 0  !important;
}
.rain{
  margin-left: 470px;
}
.sunny{
  margin-left: 0 !important;
}
}


/* NIGHT */
/* .night{
  background-color: rgb(37, 36, 36);
  height: 170px;
  width: 180px;
  display: flex;
  align-content: center;
  justify-content: center;
  border-radius: 2px;
}

.night div{
  animation-name: night;
  animation-duration: 4s;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
  width: 100px;
  height: 110px;
  border-radius: 100%;
  background-color:transparent;
  box-shadow: -23px 0 0px -10px #fff;
  margin-right: -65px;
  margin-top: 5px;
}

@keyframes night{
  0%{
    transform: rotate(-30deg);
  }
  25%{
    transform: translateY(5px);
    transform: rotate(-30deg);
  }
  50%{
    transform: translateY(10px);
  }
  100%{
    transform: translateY(0px);
    transform: rotate(-30deg);
  }
}

.sunny{
  background-color: #4fc1e9;
  height: 170px;
  width: 180px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.sunny div{
  animation-name: sunny;
  animation-duration: 4s;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
  width: 90px;
  height: 90px;
  border-radius: 100%;
  background-color: #ffe400;
  box-shadow: rgb(255 255 0 / 10%) 0 0 5px 5px;
}
@keyframes sunny{
  0%{
    transform: translateY(0px);
  }
  25%{
    transform: translateY(-8px);
  }
  50%{
    transform: translateY(8px);
  }
  100%{
    transform: translateY(-0px);
  }
} */


.x2 { 
	-webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-left: 470px;
}
.cloud {
	background: #ccc;
	background: -moz-linear-gradient(top,  #fff 5%, #f1f1f1 100%);
	background: -webkit-gradient(linear, left top, left bottom, color-stop(5%,#fff), color-stop(100%,#f1f1f1));
	background: -webkit-linear-gradient(top,  #fff 5%,#f1f1f1 100%);
	background: -o-linear-gradient(top,  #fff 5%,#f1f1f1 100%);
	background: -ms-linear-gradient(top,  #fff 5%,#f1f1f1 100%);
	background: linear-gradient(top,  #fff 5%,#f1f1f1 100%);
	filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#fff', endColorstr='#f1f1f1',GradientType=0 );
	
	-webkit-border-radius: 100px;
	-moz-border-radius: 100px;
	border-radius: 100px;
	
	-webkit-box-shadow: 0 8px 5px rgba(0, 0, 0, 0.1);
	-moz-box-shadow: 0 8px 5px rgba(0, 0, 0, 0.1);
	box-shadow: 0 8px 5px rgba(0, 0, 0, 0.1);

	height: 120px;
	position: relative;
	width: 350px;
}

.cloud:after, .cloud:before {
  background: #ccc;
	content: '';
	position: absolute;
	z-indeX: -1;
}

.cloud:after {
	-webkit-border-radius: 100px;
	-moz-border-radius: 100px;
	border-radius: 100px;

	height: 100px;
	left: 50px;
	top: -50px;
	width: 100px;
}

.cloud:before {
	-webkit-border-radius: 200px;
	-moz-border-radius: 200px;
	border-radius: 200px;

	width: 180px;
	height: 180px;
	right: 50px;
	top: -90px;
}

.cloud span{
  width: 1px;
  height: 6px;

  animation: rain 0.65s linear infinite;
}


/* animate the drops*/



.sunny{
    animation: sun 9s linear infinite;
    background: #ffe400;
    border-radius: 100%;
    box-shadow: rgb(255 255 0 / 10%) 0 0 30px 30px;
    display: flex;
    height: 90px;
    width: 90px;
    text-align: center;
    margin-top: 20px;
    margin-bottom: 20px;
    margin-left: 600px;
}

/* RAIN */

.rain{
  -webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
  margin-right: -400px;
}
.rain-block {
	background: #ccc;
	background: linear-gradient(top,  #fff 5%,#f1f1f1 100%);	
	border-radius: 100px;
  display: flex;
	box-shadow: 0 8px 5px rgba(0, 0, 0, 0.1);
	height: 120px;
	position: relative;
	width: 350px;
  justify-content: space-between;
}

.rain-block:after, .rain-block:before {
  background: #ccc;
	content: '';
	position: absolute;
	z-indeX: -1;
}

.rain-block:after {
	-webkit-border-radius: 100px;
	-moz-border-radius: 100px;
	border-radius: 100px;

	height: 100px;
	left: 50px;
	top: -50px;
	width: 100px;
}

.rain-block:before {
	-webkit-border-radius: 200px;
	-moz-border-radius: 200px;
	border-radius: 200px;

	width: 180px;
	height: 180px;
	right: 50px;
	top: -90px;
}

.rain-{
  width: 10px;
  height: 50px;
  background-color: rgb(79, 132, 182);
  animation: rain 2s linear infinite;
  margin-top: 100px;
  border-radius: 10px;
}

@keyframes rain {
  0%{
    margin-top: 10px;
    opacity: 0;
  }
  25%{
    margin-top: 100px;
    opacity: 1;
  }
  50%{
    margin-top: 150px;
    opacity: 1;
  }
  75%{
    margin-top: 200px;
    opacity: 0; 
  }
  100%{
    opacity: 0;
  }
}

.snow{
  -webkit-transform: scale(0.3);
	-moz-transform: scale(0.3);
	transform: scale(0.3);
}
.snow-block {
	background: #fff;
	background: linear-gradient(top,  #fff 5%,#f1f1f1 100%);	
	border-radius: 100px;
  display: flex;
	box-shadow: 0 8px 5px rgba(0, 0, 0, 0.1);
	height: 120px;
	position: relative;
	width: 350px;
  justify-content: space-between;
}

.snow-block:after, .snow-block:before {
  background: #fff;
	content: '';
	position: absolute;
	z-indeX: -1;
}

.snow-block:after {
	-webkit-border-radius: 100px;
	-moz-border-radius: 100px;
	border-radius: 100px;

	height: 100px;
	left: 50px;
	top: -50px;
	width: 100px;
}

.snow-block:before {
	-webkit-border-radius: 200px;
	-moz-border-radius: 200px;
	border-radius: 200px;

	width: 180px;
	height: 180px;
	right: 50px;
	top: -90px;
}

.snow-{
  width: 10px;
  height: 30px;
  background-color: #fff;
  animation: rain 2s linear infinite;
  margin-top: 100px;
  border-radius: 10px;
}

@keyframes rain {
  0%{
    margin-top: 10px;
    opacity: 0;
  }
  25%{
    margin-top: 100px;
    opacity: 1;
  }
  50%{
    margin-top: 150px;
    opacity: 1;
  }
  75%{
    margin-top: 200px;
    opacity: 0; 
  }
  100%{
    opacity: 0;
  }
}
</style>

