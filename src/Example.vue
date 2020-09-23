<template>
  <div id="app">
    <gstc :config="config" @state="onState" @loaded="onLoaded" />
  </div>
</template>

<script>
import GSTCComponent, { GSTC } from './components/GSTC.vue';
let subs = [];

const GSTCID = GSTC.api.GSTCID;
const columns = {
  data: {
    [GSTCID('id')]: {
      id: GSTCID('id'),
      data: ({ row }) => GSTC.api.sourceID(row.id),
      width: 80,
      sortable: ({ row }) => Number(GSTC.api.sourceID(row.id)),
      header: {
        content: 'ID',
      },
    },
    [GSTCID('label')]: {
      id: GSTCID('label'),
      data: 'label',
      sortable: 'label',
      isHTML: false,
      width: 230,
      header: {
        content: 'Label',
      },
    },
  },
};

const rows = {
  [GSTCID('1')]: {
    id: GSTCID('1'),
    label: 'Row #1',
  },
  [GSTCID('2')]: {
    id: GSTCID('2'),
    label: 'Row #2',
  },
};

const items = {
  [GSTCID('1')]: {
    id: GSTCID('1'),
    label: 'Item #1',
    rowId: GSTCID('1'), // row id
    time: {
      start: GSTC.api.date('2020-01-01').valueOf(),
      end: GSTC.api.date('2020-01-02').valueOf(),
    },
  },
  [GSTCID('2')]: {
    id: GSTCID('2'),
    label: 'Item #2',
    rowId: GSTCID('2'),
    time: {
      start: GSTC.api.date('2020-02-01').valueOf(),
      end: GSTC.api.date('2020-02-02').valueOf(),
    },
  },
  [GSTCID('3')]: {
    id: GSTCID('3'),
    label: 'Item #3',
    rowId: GSTCID('2'),
    time: {
      start: GSTC.api.date('2020-02-10').valueOf(),
      end: GSTC.api.date('2020-02-12').valueOf(),
    },
  },
};

const config = {
  licenseKey:
    '====BEGIN LICENSE KEY====\nXOfH/lnVASM6et4Co473t9jPIvhmQ/l0X3Ewog30VudX6GVkOB0n3oDx42NtADJ8HjYrhfXKSNu5EMRb5KzCLvMt/pu7xugjbvpyI1glE7Ha6E5VZwRpb4AC8T1KBF67FKAgaI7YFeOtPFROSCKrW5la38jbE5fo+q2N6wAfEti8la2ie6/7U2V+SdJPqkm/mLY/JBHdvDHoUduwe4zgqBUYLTNUgX6aKdlhpZPuHfj2SMeB/tcTJfH48rN1mgGkNkAT9ovROwI7ReLrdlHrHmJ1UwZZnAfxAC3ftIjgTEHsd/f+JrjW6t+kL6Ef1tT1eQ2DPFLJlhluTD91AsZMUg==||U2FsdGVkX1/SWWqU9YmxtM0T6Nm5mClKwqTaoF9wgZd9rNw2xs4hnY8Ilv8DZtFyNt92xym3eB6WA605N5llLm0D68EQtU9ci1rTEDopZ1ODzcqtTVSoFEloNPFSfW6LTIC9+2LSVBeeHXoLEQiLYHWihHu10Xll3KsH9iBObDACDm1PT7IV4uWvNpNeuKJc\npY3C5SG+3sHRX1aeMnHlKLhaIsOdw2IexjvMqocVpfRpX4wnsabNA0VJ3k95zUPS3vTtSegeDhwbl6j+/FZcGk9i+gAy6LuetlKuARjPYn2LH5Be3Ah+ggSBPlxf3JW9rtWNdUoFByHTcFlhzlU9HnpnBUrgcVMhCQ7SAjN9h2NMGmCr10Rn4OE0WtelNqYVig7KmENaPvFT+k2I0cYZ4KWwxxsQNKbjEAxJxrzK4HkaczCvyQbzj4Ppxx/0q+Cns44OeyWcwYD/vSaJm4Kptwpr+L4y5BoSO/WeqhSUQQ85nvOhtE0pSH/ZXYo3pqjPdQRfNm6NFeBl2lwTmZUEuw==\n====END LICENSE KEY====',
  innerHeight: 100,
  list: {
    columns,
    rows,
  },
  chart: {
    items,
  },
};

export default {
  name: 'Example',
  components: {
    gstc: GSTCComponent,
  },
  data() {
    return {
      config,
    };
  },
  methods: {
    onState(state) {
      this.state = state;
      subs.push(
        state.subscribe(`config.chart.items.${GSTCID('1')}`, (item) => {
          console.log('item 1 changed', item);
        })
      );
      subs.push(
        state.subscribe(`config.list.rows.${GSTCID('1')}`, (row) => {
          console.log('row 1 changed', row);
        })
      );
    },
    onLoaded(gstc) {
      console.log('gstc loaded!', gstc);
      window.gstc = gstc; // you can console it out
      setTimeout(() => {
        const item1 = this.config.chart.items[GSTCID('1')];
        item1.label = 'label changed dynamically';
        item1.time.end += 2 * 24 * 60 * 60 * 1000;
        const row1 = this.config.list.rows[GSTCID('1')];
        row1.label = 'label changed dynamically';
      }, 4000);
    },
  },
  beforeUnmount() {
    subs.forEach((unsub) => unsub());
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
