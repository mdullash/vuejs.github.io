<template>
  <div id="app" :class="typeof weather.main!='undefined' && weather.main.temp > 20 ? 'warm': ''" >
    <main>
    <div class="search">
      <input type="text" @keypress="searchData" class="locationsearch" v-model="query" placeholder="Enter Location">
    </div>
    <div v-if="typeof weather.main != 'undefined'">
      <div class="locationname">
      <h1>{{weather.name}},{{weather.sys.country}}</h1>
    </div>
    <div class="date">{{dateBuilder()}}</div>
    <div class="temparature">
    <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
    </div>
    <div class="status">{{weather.weather[0].main}}</div>
    </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
  return{
    api_key:'0b3f6e5fc365f8477251df1d1e907991',
    url_base:'https://api.openweathermap.org/data/2.5/',
    query:'',
    weather:{}
    }
  },
  methods: {
    searchData(e){
      if(e.key=='Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res=> {
            console.log(res)
            return res.json()
        }).then(this.setResults)
    }
  },
  setResults(results){
    this.weather=results;
  },
  dateBuilder(){
      let d=new Date();
      let days=["sunday","monday","tuesday","wednesday","thursday","friday","satusday"];
      let months=["January","February","March","April","May","June","July","August","Sepetember","October","November","December"];

      let day=days[d.getDay()]
      let date=d.getDate()
      let month=months[d.getMonth()]
      let year=d.getFullYear()
      return `${day} ${date} ${month} ${year}`

    }
}
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border: 0;
  outline: 0;
  transition: 0.5s;
}
#app{
  background-image: url('assets/cold.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  min-height: 100vh;
}
#app.warm{
  background-image: url('assets/warm.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  min-height: 100vh;
}
main{
    background-image: linear-gradient(rgba(0,0,0,0.4),rgba(0,0,0,0.4));
    background-size:cover;
    min-height: 100vh;
}
.search{
  padding: 20px;
}
.search .locationsearch{
  width: 100%;
  padding: 10px;
  border-radius: 0px 10px 0px 10px;
}
.locationsearch:hover{
  width: 100%;
  padding: 10px;
  border-radius: 10px 0px 10px 0px;
  background-color: aqua;
}
.locationname{
  color: white;
  text-align: center;
}
.date{
  text-align: center;
  font-style: italic;
  margin-top: 0.5rem;
  color: rgb(223, 248, 0)
}
.temparature{
  text-align: center;
}
.temparature .temp{
  display: inline-block;
  background-color: white;
  border-radius: 5px;
  padding: 30px 60px;
  margin-top: 20px;
  font-weight: 600;
  font-size: 80px;
  font-style: italic;
  box-shadow: 5px 5px 5px rgb(44, 112, 143)
}
.status{
  text-align: center;
  margin-top: 30px;
  font-weight: 600;
  font-size: 30px;
  font-style: italic;
  color: rgb(241, 21, 32);
}
</style>
