
<template>
  <div>
    <label :for="id" tabindex="0">{{ label }}</label>
    <input
      :id="id"
      :name="id"
      type="number"
      v-model="hour"
      @change="$emit('update:hour', $event.target.value)"
      v-bind="$attrs"
      min="0"
    />
    <small>H</small>
    <input
      :id="id"
      :name="id"
      type="number"
      v-model="minute"
      @change="$emit('update:minute', $event.target.value)"
      v-bind="$attrs"
      min="0"
      max="59"
    />
    <small>M</small>
  </div>
</template>

<script>
export default {
  name: "TimeInput",
  inheritAttrs: false,
  data() {
    return {
      hour: null,
      minute: null,
    };
  },
  emits: ["update:hour", "update:minute"],
  props: {
    id: String,
    value: null,
    label: String,
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