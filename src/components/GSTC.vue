<template>
  <div class="gstc" ref="gstc"></div>
</template>

<script>
import GSTC from "gantt-schedule-timeline-calendar";
import "gantt-schedule-timeline-calendar/dist/style.css";

let state, gstc;

export default {
  name: "GSTC",
  props: ["config"],
  mounted() {
    state = GSTC.api.stateFromConfig(this.config);
    this.$emit("state", state);
    gstc = GSTC({
      element: this.$refs.gstc,
      state
    });
    this.$emit('ready', gstc);
    this.$watch(
      "config",
      config => {
        state.update("config", current => {
          if (current !== config) {
            return GSTC.api.mergeDeep({}, current, config);
          } else {
            return current;
          }
        });
      },
      { deep: true }
    );
  },
  destroyed() {
    gstc.app.destroy();
  }
};
</script>
