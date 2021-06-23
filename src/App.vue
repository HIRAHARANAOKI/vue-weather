<template>
    <div id="app"
    :class="backgroundImageClass"
    >
        <main>
            <h2 class='weather-title'>世界の天気を検索するアプリ</h2>
            <div class="search-box">
                <input type="text" class="search-bar" placeholder="Search..."
                v-model="query"
                v-on:keypress="fetchWeather"
                 /> 
            </div>
            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country}}</div>
                    <div class="date">{{ dateBuilder() }}</div>
                </div>

                <div class="weather-box">
                    <div class="temp">{{ Math.round(weather.main.temp) }}℃</div>
                    <div class="weather">{{ weather.weather[0].main}}</div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'app',
    data() {
        return {
            api_key: process.env.VUE_APP_API_KEY,
            url_base: 'https://api.openweathermap.org/data/2.5/',
            query: '',
            weather: {},
            backgroundImage: ''
        }
    },
    // 天気ごとにclassを付与
    computed: {
        backgroundImageClass () {
            if (this.backgroundImage === 'Clouds') {
                return 'cloud'
           } else if (this.backgroundImage === 'Clear') {
                return 'clear'
           } else if (this.backgroundImage === 'Rain') {
               return 'rain'
           } else {
               return ''
           }
        }
    },
    methods: {
        // axiosでopenweatherapiにアクセス
        fetchWeather (e) {
            if (e.key == "Enter") {
                axios.get(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                .then(res => {
                    return res.data
                }).then(this.setResults)
                .catch((error) => console.log(error))
            }
        },
        setResults (results) {
            // weatherへ格納
            this.weather = results;
            // backgroundへ格納
            this.backgroundImage = results.weather[0].main
        },
        // 日付処理
        dateBuilder () {
            let d = new Date();
            let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            let days =["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Thursday", "Friday", "Saturday"]
            
            let day = days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();

            return `${day} ${date} ${month} ${year}`;
        }
    },
}
</script>
<style>
  @charset "UTF-8";
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("https://images.unsplash.com/photo-1534068731687-d70176c2e7d5?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app .weather-title {
  text-align: center;
  padding: 20px;
  font-size: 40px;
}
#app.sun {
  background-image: url("https://images.unsplash.com/photo-1541606792286-5aa5bba062fc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&/ixlib=rb-1.2.1&auto=format&fit=crop&w=668&q=80");
}
#app.clear {
  background-image: url("https://images.unsplash.com/photo-1610289303105-286fc12c6183?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=934&q=80");
}
#app.rain {
  background-image: url("https://images.unsplash.com/photo-1501691223387-dd0500403074?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=882&q=80");
}
#app.cloud {
  background-image: url("https://images.unsplash.com/photo-1591804227855-d6fe0b648d0e?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=922&q=80");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  /* デフォルトのスタイルを無効フォーム関連のタグに向いている */
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  border-radius: 0px 16px 0px 16px;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: white;
  border-radius: 16px 0px 16px 0px;
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
  background-color: rgba(255, 255, 255, 0.25);
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
</style>