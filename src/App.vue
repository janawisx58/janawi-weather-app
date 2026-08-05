
<script setup>
import { ref } from 'vue'

const api_key = ref('db158eedefeefb124c54d04e046ad6ad')
const url_base = ref('https://api.openweathermap.org/data/2.5/')
const query = ref('')
const weather = ref({})

// دالة جلب البيانات من الـ API
const fetchWeather = (e) => {
  if (e.key === 'Enter' && query.value.trim() !== '') {
    fetch(`${url_base.value}weather?q=${query.value}&units=metric&APPID=${api_key.value}`)
      .then(res => res.json())
      .then(results => {
        weather.value = results
        query.value = ''
      })
      .catch(err => console.error("Error fetching weather:", err))
  }
}

// دالة تنسيق التاريخ
const dateBuilder = () => {
  const d = new Date()
  const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
  const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

  const day = days[d.getDay()]
  const date = d.getDate()
  const month = months[d.getMonth()]
  const year = d.getFullYear()

  return `${day} ${date} ${month} ${year}`
}
</script>







<template>
  <main class="app-container">
    <div class="search-box">
      <input 
        type="text" 
        class="search-bar" 
        placeholder="Search for a city..." 
        v-model="query"
        @keypress="fetchWeather"
      />
    </div>

    <!-- بطاقة عرض الطقس -->
    <div class="weather-wrap" v-if="weather.main">
      <div class="location-box">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        <div class="date">{{ dateBuilder() }}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
  </main>
    <footer class="cute-footer">
  💕 made by <span>little coder janawi</span> 💕
</footer>
</template>












<style>
/* تصفير التنسيقات وتحديد الخواص الأساسية */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Poppins', 'Segoe UI', Roboto, sans-serif;
  background: linear-gradient(135deg, #ffd1dc 0%, #ffe6eb 50%, #e8dff5 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #4a4a4a;
}

/* الحاوية الرئيسية للتطبيق */
.app-container {
  width: 100%;
  max-width: 420px;
  min-height: 560px;
  margin: 20px;
  padding: 30px 25px;
  background: rgba(255, 255, 255, 0.45);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-radius: 28px;
  border: 1px solid rgba(255, 255, 255, 0.6);
  box-shadow: 0 20px 40px rgba(244, 168, 191, 0.25);
  transition: all 0.4s ease;
}

/* شريط البحث */
.search-box {
  width: 100%;
  margin-bottom: 35px;
}

.search-bar {
  width: 100%;
  padding: 16px 20px;
  color: #5c5c5c;
  font-size: 16px;
  appearance: none;
  border: none;
  outline: none;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 16px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.03);
  transition: all 0.3s ease-in-out;
}

.search-bar::placeholder {
  color: #b5a4b7;
}

.search-bar:focus {
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 10px 25px rgba(255, 182, 193, 0.4);
  transform: translateY(-2px);
}

/* تفاصيل الموقع والتاريخ */
.location-box {
  text-align: center;
  margin-bottom: 25px;
}

.location-box .location {
  color: #4a3e4e;
  font-size: 28px;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.location-box .date {
  color: #8c7a92;
  font-size: 14px;
  font-weight: 400;
  margin-top: 6px;
}

/* بطاقة الحرارة وحالة الطقس */
.weather-box {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 20px 35px;
  color: #ffffff;
  font-size: 64px;
  font-weight: 900;
  text-shadow: 2px 4px 10px rgba(0, 0, 0, 0.08);
  background: linear-gradient(135deg, #ff9ebb 0%, #ffb6c1 100%);
  border-radius: 24px;
  box-shadow: 0 12px 30px rgba(255, 158, 187, 0.4);
  margin: 15px 0;
  transition: transform 0.3s ease;
}

.weather-box .temp:hover {
  transform: scale(1.03);
}

.weather-box .weather {
  color: #5a4b5d;
  font-size: 22px;
  font-weight: 600;
  letter-spacing: 1px;
}

/* التجاوب مع شاشات الجوال الصغرى */
@media (max-width: 480px) {
  .app-container {
    margin: 10px;
    padding: 25px 18px;
    border-radius: 20px;
  }

  .weather-box .temp {
    font-size: 52px;
    padding: 15px 28px;
  }

  .location-box .location {
    font-size: 24px;
  }
} 
.cute-footer {
  margin-top: 30px;
  text-align: center;
  font-size: 0.9rem;
  color: #6b4654;
  font-weight: 500;
  letter-spacing: 0.5px;
  opacity: 0.9;
  transition: transform 0.3s ease;
}

.cute-footer span {
  font-weight: 700;
  color: #ff6b81;
  background: linear-gradient(135deg, #ff758c, #ff7eb3);
  -webkit-text-fill-color: transparent;
}

.cute-footer:hover {
  transform: scale(1.05);
}
</style>