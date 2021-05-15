<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" alt="" class="w-40 m-auto" />
  </main>
</template>

<script>
import DataTitle from "../components/DataTitle";
import DataBoxes from "../components/DataBoxes";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: "",
      countries: [],
      loadingImage: require("../assets/loading.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
    },
  },
  async created() {
    const data = await this.fetchCovidData();
    console.log(data);

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>
