<template>
  <div id="app">
    <form>
      <label v-for="year in years" :key="year">
        {{ year }}
        <input type="checkbox" v-model="selectedYears" :value="year" />
      </label>
    </form>
    <div v-for="year in selectedYears" :key="year">
      <h3>Year {{ year }}</h3>
      <select v-model="selectedPeriods[year]" @change="displayInputFields(year)">
        <option value="">Select a period</option>
        <option value="Jan-Apr">Jan - Apr</option>
        <option value="May-Aug">May - Aug</option>
        <option value="Sep-Dec">Sep - Dec</option>
      </select>
      <div v-if="selectedPeriods[year]">
        <input type="number" v-for="(month, index) in monthNames[year]" :key="index" v-model="values[year][index]" @input="disableInputFields(year, index)" />
        <button @click="calculateValues(year)">Calculate</button>
        <button @click="resetValues(year)">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      years: [2021, 2022, 2023],
      selectedYears: [],
      selectedPeriods: {},
      monthNames: {},
      values: {},
    };
  },
  methods: {
    displayInputFields(year) {
      const period = this.selectedPeriods[year];
      this.monthNames[year] = period.split("-");
      this.values[year] = Array(this.monthNames[year].length).fill(0);
    },
    disableInputFields(year, index) {
      const value = this.values[year][index];
      if (value !== 0) {
        this.values[year] = this.values[year].map((v, i) => (i === index ? v : null));
      }
    },
    calculateValues(year) {
      this.values[year] = this.values[year].map((value) => {
        if (value === null) {
          const randomValue = Math.floor(Math.random() * 200) - 100 + value;
          return randomValue;
        }
        return value;
      });
    },
    resetValues(year) {
      this.selectedPeriods[year] = "";
      this.monthNames[year] = [];
      this.values[year] = [];
    },
  },
};
</script>

<style>
.input-field {
  width: 50px;
  margin-right: 10px;
}
</style>
