<template>
  <div id="app">
    <h1 tabindex="0">WORKTIME CALCULATOR</h1>
    <form>
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
      />
      <TimeInput
        id="awayTime"
        label="How long were you away from work?"
        v-model:hour="awayTimeHour"
        v-model:minute="awayTimeMinute"
      />
      <button type="button" @click="calculateWorkDay">Calculate</button>
    </form>
    <div id="result-wrapper" tabindex="0">
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
      console.log("TimeObj: ", time);
      this[key] = time;
    },
    calculateWorkDay() {
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
      console.log("tH", totalHours);

      if (totalHours < 10) {
        totalHours = `0${totalHours}`;
      }

      if (totalMinutes < 10) {
        totalMinutes = `0${totalMinutes}`;
      }

      if (daysToCarry === 1) {
        this.result = `Your work day ends in ${daysToCarry} day @ ${totalHours}:${totalMinutes}`;
      } else if (daysToCarry < 1) {
        this.result = `Your work day ends @ ${totalHours}:${totalMinutes}. Yippee ki yay!`;
      } else {
        this.result = `Your work day ends in ${daysToCarry} days @ ${totalHours}:${totalMinutes}. Yikes..`;
      }
    },
  },
};
</script>

<style>
@import './styles/style.css';

</style>
