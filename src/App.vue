<script setup>

</script>

<script>
export default {
 name:'App',
  data(){
   return {
     api_key:'afe37aa150337f1d4d15d6af1fc109e2',
     url_base:"https://api.openweathermap.org/data/2.5/",
     query:'',
     weather:{},
     errorMessage:false
   }
  },
  methods:{
 async fetchWeather (e) {
   if (e.key === 'Enter' && this.query !== '') {
     try {
      const resp = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
       if(resp.status === 404){
         this.setErrors()
         this.weather = {}
         return
       }
       const results = await  resp.json();
       this.setResults(results)
     } catch (err) {
       console.log(err)
     }

   }
 }
   ,
    setResults(results){
      this.weather = results
      this.errorMessage = false
    },
    setErrors(err){
      this.errorMessage = true
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  },
  mounted() {
    fetch(`${this.url_base}weather?q=Georgia&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        })
        .then(this.setResults)
        .catch(err => this.setErrors())
  }

}
</script>





<template>
<div id="App" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : typeof weather.main !== 'undefined' && weather.main.temp < -20 ? 'frost': ''"  >
  <main>
    <div class="search-box">
      <input type="text"
             class="search-bar"
             placeholder="search..."
             v-model="query"
             @keypress="fetchWeather"
      >

    </div>

    <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
      <div class="location-box">
        <div class="location">{{weather.name}}  ,  {{weather.sys.country}}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}??c</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>
    <div v-if="errorMessage" class="error">No Country Found!</div>

  </main>
</div>
</template>






<style scoped>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family:'montserrat',sans-serif;
}

#App{
  background-image: url('./assets/sunny.jpg');
  background-size: cover;
  background-position: bottom;
  transition: all 0.4s;
}
#App.warm {
  background-image: url('./assets/desert.webp');
}
#App.frost{
  background-image: url('./assets/frost.jpg');
}
main{
  min-height: 100vh;
  padding:25px 25px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.35),rgba(0,0,0,0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.5) ;
  border-radius: 16px 16px 16px 16px;
  transition: all 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.75) ;
  border-radius: 16px 16px 16px 16px;

}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.error{
  font-size: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #c1b4b4;
  font-family: sans-serif;
}

@media  all and (min-width:768px) {
  main {
    padding: 25px 70px;
  }

}

@media  all and (min-width:1120px) {
  main {
    padding: 25px 160px;
  }

}
@media  all and (min-width:1420px) {
  main {
    padding: 25px 360px;
  }

}





</style>
