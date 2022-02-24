<template>
  <div class="firstcard">
      <h3>Weather on  {{city}} for this week </h3>
    </div>
  <div class="weather">
    <div v-for="(data, index) in allDay" :key="index" class="card">
      <span class="firstcard">
        {{ Math.floor(allDay[index].main.temp) }}&deg;
      </span>
      <span>
        <img
          class="city-icon"
          :src="`http://openweathermap.org/img/w/${allDay[index].weather[0].icon}.png`"
        />
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      weatherData: [],
      date: new Date(),
      allDay: [],
      nowDate: "",
      markup: "",
      city: "",
    };
  },

  mounted() {
    this.fetchData();
  },
  methods: {
    /**
     * Fetch Data From The Given API
     */
    fetchData() {
      fetch(
        "https://api.openweathermap.org/data/2.5/forecast?q=amman&appid=aa73d3feb388d4f8c2e2bd8ce49977ab&units=metric"
      )
        .then((resp) => resp.json())
        .then((data) => this.fliterOneWeekData(data));
    },

    /**
     * Filtering A week Data
     * @param {raw Data from the api} data
     */
    fliterOneWeekData(data) {
      this.city = data.city.name;
      this.nowDate = this.date.getDate();
      for (let i = 1; i < 6; i++) {
        this.allDay = data.list.filter((item) =>
          item.dt_txt.includes(`${this.date.getMonth() + 1}-${this.nowDate}`)
        );
        this.nowDate++;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.firstcard {
  font-size: 25px;
  font-family: 'Times New Roman', Times, serif;
  display: flex;
  flex-direction: row;
  text-align: center;
  padding: 12px 2px;
}

h3 {
  display: flex;
  padding: 15px 12px;
  flex-direction: row;
  justify-content: flex-start;
  background-color: #f5ebeb;
  overflow-x: hidden;
  transition: 0.3s;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: 0 auto;
  margin-bottom: 40px;
  border-radius: 12px;
  max-width: 400px;
  }

.card {
  align-items: center;
  display: flex;
  justify-content: space-between;
  font-size: 18px;
  font-family: "Space Grotesk";
  flex-direction: column;
}

.weather {
  display: flex;
  padding: 15px 12px;
  flex-direction: row;
  justify-content: flex-start;
  background-color: #f5ebeb;
  overflow-x: hidden;
  transition: 0.3s;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  margin: 0 auto;
  margin-bottom: 40px;
  border-radius: 12px;
  max-width: 400px;
}
</style>
