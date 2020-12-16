<template>
  <div>
    <label :for="id" tabindex="0">{{ label }}</label>
    <input
      :id="id"
      :name="id"
      type="time"
      v-bind="$attrs"
      @input="handleTimeInput"
    />
  </div>
</template>

<script>
export default {
  name: "TimePickerInput",
  inheritAttrs: false,
  data() {
    return {
      timeParams: null,
    };
  },
  emits: ["set-start-time"],
  props: {
    id: String,
    value: null,
    label: String,
  },
  methods: {
    handleTimeInput(e) {
      const timeParamsSplit = e.target.value.split(":");
      const timeParamsInt = timeParamsSplit.map(convertToInt);
      const timeParams = {
        hour: timeParamsInt[0],
        minute: timeParamsInt[1],
      };

      function convertToInt(time) {
        return parseInt(time);
      }

      this.$emit("set-start-time", timeParams, this.id);
    },
  },
};
</script>

<style scoped>
label {
  display: block;
  margin-bottom: 1rem;
}

input {
  background-color: var(--surface-color);
  border-bottom: 2px solid var(--primary-contrast);
  border-right: 2px solid var(--primary-contrast);
  border-radius: var(--br-medium);
  color: var(--on-surface-color);
  padding: 0.8rem;
  width: max-content;
}
</style>