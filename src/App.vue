<template>
  <v-app>
    <v-main>
      <v-text-field label="Ime" @change="getAll($event)" />
      <v-data-table
        :headers="headers"
        :items="processedData"
        :items-per-page="9"
        class="elevation-1"
      ></v-data-table>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    ageData: {},
    genderData: {},
    nationalityData: {},

    unprocessedData: [],
    processedData: [],

    headers: [
      {
        text: "Ime",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Država", value: "country" },
      { text: "Vjerojatnost države", value: "probability of country" },
      { text: "Godine", value: "age" },
      { text: "Spol", value: "gender" },
      { text: "Vjerojatnost spola", value: "probability of gender" },
    ],
  }),

  methods: {
    getAge(value) {
      fetch("https://api.agify.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.ageData = data));
    },
    getGender(value) {
      fetch("https://api.genderize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.genderData = data));
    },
    getNationality(value) {
      fetch("https://api.nationalize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.nationalityData = data));
    },
    getAll(value) {
      this.getAge(value);
      this.getGender(value);
      this.getNationality(value);

      this.unprocessedData = [
        this.ageData,
        this.genderData,
        this.nationalityData,
      ];

      let halfProcessedData = [];

      this.unprocessedData = this.unprocessedData[2].country.forEach(
        (country) =>
          halfProcessedData.push([
            this.unprocessedData[0],
            this.unprocessedData[1],
            country,
          ])
      );

      halfProcessedData.forEach((array) =>
        this.processedData.push({
          name: array[0].name,
          age: array[0].age,
          gender: array[1].gender,
          "probability of gender": array[1].probability,
          country: array[2].country_id,
          "probability of country": array[2].probability,
        })
      );
    },
  },
};
</script>
