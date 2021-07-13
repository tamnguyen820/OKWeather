<style src="./styles.css"></style>
<template src="./template.html"></template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "e4da7390d164b338b8a1eceaed68e493",
      url_base: "https://api.openweathermap.org/data/2.5/",
      call: "",
      weather: {},
    };
  },
  methods: {
    getWeather() {
      fetch(
        `${this.url_base}weather?q=${this.call}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },
    setResults(results) {
      if (results.cod !== 200) {
        alert("error " + results.cod + ": " + results.message);
      } else {
        this.weather = results;
      }
    },
    createDate() {
      // Convert to location's date
      var systemDate = new Date();
      var locationDate = new Date(
        new Date().getTime() +
          1000 * this.weather.timezone +
          systemDate.getTimezoneOffset() * 60000
      );
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      // Determine time based on the 12 hour system
      // Change title and icon if needed
      var twelveSystem = "a.m.";
      var hour = locationDate.getHours();
      var timeInDay;
      document.getElementById("okw").className = "title";
      if (hour >= 5 && hour <= 8) {
        timeInDay = "dawn";
      } else if (hour >= 9 && hour <= 16) {
        timeInDay = "day";
      } else if (hour >= 17 && hour <= 20) {
        timeInDay = "dusk";
      } else {
        timeInDay = "night";
        document.getElementById("okw").className = "title-night";
        this.weather["base"] = "night";
      }
      if (hour >= 12) {
        twelveSystem = "p.m.";
        hour -= 12;
      }
      if (hour == 0) {
        hour += 12;
      }

      // Change background image based on time
      document.getElementById("app").className = timeInDay;

      // Format the return date and time
      var minute = locationDate.getMinutes();
      var extraMinuteDigit = "";
      if (minute < 10) {
        extraMinuteDigit = "0";
      }
      var day = days[locationDate.getDay()];
      var date = locationDate.getDate();
      var month = months[locationDate.getMonth()];
      var year = locationDate.getFullYear();

      return `${day}, ${month} ${date}, ${year} | ${hour}:${extraMinuteDigit}${minute} ${twelveSystem} `;
    },
  },
};
</script>
