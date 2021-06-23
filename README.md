# 世界の天気情報を検索するWEBアプリ

## 環境

- Vue
- axios
- OpenWeatherAPI

### 工夫したこと

天気（rain、cloud、clear）によって背景の壁紙が変化するようにしました。


下記のようにdataにbackgroundImage: ''を記載
```
export default {
  data() {
    return {
      backgroundImage: '' 
     }
  }
}
```

class名が動的に変化するように記載
```
  <div id="app"
    :class="backgroundImageClass"
    >
```

computedで条件分岐を記載
```
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
```











