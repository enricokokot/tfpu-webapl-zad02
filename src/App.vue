<template>
  <v-app>
    <v-main>
      <v-text-field label="Name" @change="getAll($event)" />
      <v-data-table />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),

  methods: {
    getAge(value) {
      fetch("https://api.agify.io?name=" + value)
        .then((response) => response.json())
        .then((data) => console.log(data));
    },
    getGender(value) {
      fetch("https://api.genderize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => console.log(data));
    },
    getNationality(value) {
      fetch("https://api.nationalize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => console.log(data));
    },
    getAll(value) {
      this.getAge(value);
      this.getGender(value);
      this.getNationality(value);
    },
  },
};
</script>
