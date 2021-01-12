<template>
  <div id="app">
    <h1 tabindex="0">WORKTIME CALCULATOR</h1>
    <form v-on:submit.prevent="calculateWorkDay">
      <TimePickerInput
        id="startTime"
        label="When did you start your workday?"
        v-model:startTime="startTime"
        @setStartTime="updateTime"
      />
      <TimeInput
        id="totalTime"
        label="How many hours are you working?"
        v-model:hour="totalTimeHour"
        v-model:minute="totalTimeMinute"
        defaultTime="Default: 7h 30min"
      />
      <TimeInput
        id="awayTime"
        label="How long were you away from work?"
        v-model:hour="awayTimeHour"
        v-model:minute="awayTimeMinute"
      />
      <button type="submit">Calculate</button>
    </form>
    <div id="result-wrapper">
      <h2>RESULT:</h2>
      <p>{{ result }}</p>
    </div>
  </div>
</template>

<script>
import TimePickerInput from "./components/TimePickerInput";
import TimeInput from "./components/TimeInput";

export default {
  name: "App",
  components: {
    TimePickerInput,
    TimeInput,
  },
  data() {
    return {
      startTime: {},
      totalTimeHour: null,
      totalTimeMinute: null,
      awayTimeHour: null,
      awayTimeMinute: null,
      result: "",
    };
  },
  methods: {
    updateTime(time, key) {
      this[key] = time;
    },
    validateForm() {
      if (((this.totalTimeMinute === null) && (this.totalTimeHour === null)) || (!this.totalTimeHour && !this.totalTimeMinute)) {
        this.totalTimeHour = 7;
        this.totalTimeMinute = 30;
      } else if (!this.totalTimeHour || ((this.totalTimeHour === null) && (this.totalTimeMinute !== null))) {
        this.totalTimeHour = 0;
      } else if (!this.totalTimeMinute || ((this.totalTimeMinute === null) && (this.totalTimeHour !== null))) {
        this.totalTimeMinute = 0;
      }

      if (this.awayTimeHour === null) {
        this.awayTimeHour = 0;
      }

      if (this.awayTimeMinute === null) {
        this.awayTimeMinute = 0;
      }
    },
    calculateWorkDay() {
      this.validateForm();
      
      let totalMinutes =
        this.startTime.minute +
        parseInt(this.totalTimeMinute) +
        parseInt(this.awayTimeMinute);

      const hoursToCarry = Math.floor(totalMinutes / 60);
      totalMinutes = totalMinutes % 60;

      let totalHours =
        this.startTime.hour +
        parseInt(this.totalTimeHour) +
        parseInt(this.awayTimeHour) +
        hoursToCarry;

      const daysToCarry = Math.floor(totalHours / 24);
      totalHours = totalHours % 24;

      if (totalHours < 10) {
        totalHours = `0${totalHours}`;
      }

      if (totalMinutes < 10) {
        totalMinutes = `0${totalMinutes}`;
      }

      if (daysToCarry === 1) {
        this.result = `Your workday ends in ${daysToCarry} day @ ${totalHours}:${totalMinutes}`;
      } else if (daysToCarry < 1) {
        this.result = `Your workday ends @ ${totalHours}:${totalMinutes}. Yippee ki yay!`;
      } else {
        this.result = `Your workday ends in ${daysToCarry} days @ ${totalHours}:${totalMinutes}.`;
      }
    },
  },
};
</script>

<style>
@import './styles/style.css';
</style>