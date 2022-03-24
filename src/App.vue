<template>
  <v-app>
    <v-main>
      <v-text-field label="Ime" @change="getAll($event)" />
      <v-data-table
        :headers="headers"
        :items="newData"
        :items-per-page="5"
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
    allData: [],
    newData: [],
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

      let array1 = [];

      this.allData = [this.ageData, this.genderData, this.nationalityData];
      this.allData = this.allData[2].country.forEach((country) =>
        array1.push([this.allData[0], this.allData[1], country])
      );

      let array2 = [];

      array1.forEach((array) =>
        array2.push({
          name: array[0].name,
          age: array[0].age,
          gender: array[1].gender,
          "probability of gender": array[1].probability,
          country: array[2].country_id,
          "probability of country": array[2].probability,
        })
      );

      array2.forEach((object) => this.newData.push(object));
    },
  },
};
</script>
