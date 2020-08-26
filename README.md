<p align="center">
  <img src="https://neuronet.io/screenshots/gstc9-flat-bgw-300.png" alt="logo">
</p>
<hr />
<h1 align="center">vue-gantt-schedule-timeline-calendar</h1>

Vue version of [gantt-schedule-timeline-calendar](https://github.com/neuronetio/gantt-schedule-timeline-calendar)

Documentation can be found on original component page at [gantt-schedule-timeline-calendar](https://github.com/neuronetio/gantt-schedule-timeline-calendar)

<p align="center">
  <img src="https://neuronet.io/screenshots/appscrn.png?uniq=1" alt="gstc-logo">
</p>

## installation

`npm i vue-gantt-schedule-timeline-calendar`

## usage

```vue
<template>
  <div id="app">
    <GSTC :config="config" @state="onState" @ready="onReady"/>
  </div>
</template>

<script>
import GSTC from "vue-gantt-schedule-timeline-calendar";
let subs = [];

export default {
  name: "app",
  components: {
    GSTC
  },
  data() {
    return {
      config: {
        height: 300,
        list: {
          rows: {
            "1": {
              id: "1",
              label: "Row 1"
            },
            "2": {
              id: "2",
              label: "Row 2"
            },
            "3": {
              id: "3",
              label: "Row 3"
            },
            "4": {
              id: "4",
              label: "Row 4"
            }
          },
          columns: {
            data: {
              id: {
                id: "id",
                data: "id",
                width: 50,
                header: {
                  content: "ID"
                }
              },
              label: {
                id: "label",
                data: "label",
                width: 200,
                header: {
                  content: "Label"
                }
              }
            }
          }
        },
        chart: {
          items: {
            "1": {
              id: "1",
              rowId: "1",
              label: "Item 1",
              time: {
                start: new Date().getTime(),
                end: new Date().getTime() + 24 * 60 * 60 * 1000
              }
            },
            "2": {
              id: "2",
              rowId: "2",
              label: "Item 2",
              time: {
                start: new Date().getTime() + 4 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 5 * 24 * 60 * 60 * 1000
              }
            },
            "3": {
              id: "3",
              rowId: "2",
              label: "Item 3",
              time: {
                start: new Date().getTime() + 6 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 7 * 24 * 60 * 60 * 1000
              }
            },
            "4": {
              id: "4",
              rowId: "3",
              label: "Item 4",
              time: {
                start: new Date().getTime() + 10 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 12 * 24 * 60 * 60 * 1000
              }
            },
            "5": {
              id: "5",
              rowId: "4",
              label: "Item 5",
              time: {
                start: new Date().getTime() + 12 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 14 * 24 * 60 * 60 * 1000
              }
            }
          }
        }
      }
    };
  },
  methods: {
    onState(state) {
      this.state = state;
      subs.push(
        state.subscribe("config.chart.items.1", item => {
          console.log("item 1 changed", item);
        })
      );
      subs.push(
        state.subscribe("config.list.rows.1", row => {
          console.log("row 1 changed", row);
        })
      );
    },
    onReady(gstc) {
      const date = gstc.api.time.date().valueOf();
      gstc.api.scrollToTime(date);
    }
  },
  mounted() {
    setTimeout(() => {
      const item1 = this.config.chart.items["1"];
      item1.label = "label changed dynamically";
      item1.time.end += 2 * 24 * 60 * 60 * 1000;
    }, 2000);
  },
  beforeDestroy() {
    subs.forEach(unsub => unsub());
  }
};
</script>
```

## LICENSE

**[AGPL-3.0](https://github.com/neuronetio/gantt-schedule-timeline-calendar/blob/master/LICENSE)** if you are using it - your project **must** be AGPL-3.0 compatible.

For commercial license please contact me at neuronet.io@gmail.com
