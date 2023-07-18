<template>
  <div class="container">
    <div class="calc-container">
      <div class="date-inputs">
        <div class="input">
          <label for="day">DAY</label>
          <input type="number" id="day" placeholder="DD" v-model="day" required>
          <span class="error" v-if="!validDay">Must be a valid day</span>
          <span class="error" v-else-if="emptyInput">This field is required</span>
        </div>
        <div class="input">
          <label for="month">MONTH</label>
          <input type="number" id="month" placeholder="MM" v-model="month" required>
          <span class="error" v-if="!validMonth">Must be a valid month</span>
          <span class="error" v-else-if="emptyInput">This field is required</span>
        </div>
        <div class="input">
          <label for="year">YEAR</label>
          <input type="number" id="year" placeholder="YYYY" v-model="year" @keyup.enter="calculateAge()" required>
          <span class="error" v-if="!pastYear">Must be in the past</span>
          <span class="error" v-else-if="emptyInput">This field is required</span>
        </div>
      </div>
      <div class="separator">
        <img src="../assets/images/icon-arrow.svg" alt="calculate" @click="calculateAge()">
      </div>
      <div class="date-results">
        <h1 v-if="!dateResult">
          <span class="purple">--</span> years
        </h1>
        <h1 v-else>
          <span class="purple">{{ years }}</span> years
        </h1>
        <h1 v-if="!dateResult">
          <span class="purple">--</span> months
        </h1>
        <h1 v-else>
          <span class="purple">{{ months }}</span> months
        </h1>
        <h1 v-if="!dateResult">
          <span class="purple">--</span> days
        </h1>
        <h1 v-else>
          <span class="purple">{{ days }}</span> days
        </h1>
      </div>
    </div>
    <div class="attribution">
      Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>.
      Coded by <a href="https://darkerarcher.netlify.app/">Ayo Otutuloro</a>.
    </div>
  </div>
</template>

<script>
export default {
  name: 'AgeCalc',
  data() {
    return {
      dateResult: false,
      pastYear: true,
      validDay: true,
      validMonth: true,
      emptyInput: false,
      years: 0,
      months: 0,
      days: 0,
      day: null,
      month: null,
      year: null,
    }
  },
  methods: {
    calculateAge() {
      const day = this.day;
      const month = this.month;
      const year = this.year;

      const date = new Date();
      const currentYear = date.getFullYear();
      const currentMonth = date.getMonth() + 1;
      const currentDay = date.getDate();

      let years = 0;
      if (currentMonth > month ||
        (currentMonth == month &&
          currentDay >= day
        )
      ) {
        years = currentYear - year;
      }
      else {
        years = currentYear - year - 1;
      }

      let months = 0;
      if (currentDay >= day) {
        months = currentMonth - month;
      }
      else if (currentDay < day) {
        months = currentMonth - month - 1;
      }

      months = months < 0 ? months + 12 : months;


      let days = 0;
      let monthDays = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

      if (year % 4 == 0) {
        monthDays[1] = 29;
      }

      if (currentDay >= day) {
        days = currentDay - day;
      }
      else if (currentDay < day) {
        days = currentDay - day + monthDays[month - 1];
      }

      if (day > monthDays[month - 1]) {
        this.validDay = false;
        this.dateResult = false;
      } else {
        this.validDay = true;
      }

      if (month > 12) {
        this.validMonth = false;
        this.dateResult = false;
      } else {
        this.validMonth = true;
      }

      if (years < 0) {
        this.dateResult = false;
        this.pastYear = false;
      } else if (day === null || month === null || year === null) {
        this.dateResult = false;
        this.emptyInput = true;
      } else {
        this.dateResult = true;
        this.pastYear = true;
        this.emptyInput = false;
        this.years = years;
        this.months = months;
        this.days = days;
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,700;1,400;1,800&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background-color: rgb(219, 219, 219);
}

.container {
  height: 100%;
}

.calc-container {
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgb(255, 255, 255);
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 150px;
  padding: 2rem;
}

.date-inputs {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 20px;
  width: 100%;
}

.input {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  gap: 0.5rem;
  margin: 0 1rem;
}

label {
  color: rgb(113, 111, 111);
}

input {
  width: 200px;
  border: 1px solid rgb(113, 111, 111);
  border-radius: 5px;
  padding: 20px 10px;
  font-size: 32px;
  font-weight: 700;
}

input:invalid {
  border: 1px solid rgb(255, 87, 87);
}

.separator img {
  padding: 20px;
  border-radius: 50%;
  background-color: rgb(133, 77, 255);
  float: right;
  cursor: pointer;
}

.date-results {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
}

h1 {
  font-size: 50px;
  font-style: italic;
  font-weight: 800;
  color: rgb(20, 20, 20);
}

.purple {
  color: rgb(133, 77, 255);
}

.error {
  color: rgb(255, 87, 87);
  font-size: 12px;
  font-style: italic;
}

.attribution {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  padding: 20px;
  font-size: 12px;
  color: rgb(113, 111, 111);
  text-align: center;
  width: 100%;
}


@media (max-width: 600px) {

  .calc-container {
    width: 95%;
    padding: 10px;
  }

  .date-inputs {
    flex-direction: column;
    width: 100%;
  }

  input {
    width: 80%;
    padding: 15px 10px;
    font-size: 20px;
  }

  .separator img {
    padding: 10px;
    float: none;
  }

  .date-results {
    width: 90%;
  }

  h1 {
    font-size: 30px;
  }

  .attribution {
    padding: 5px;
  }
}

@media (max-width: 768px) {
  .calc-container {
    width: 95%;
  }
}
</style>
