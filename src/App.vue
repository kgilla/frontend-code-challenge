<template>
  <label for="maxCP" class="max-cp">
    <input type="checkbox" id="maxCP" v-on:change="handleChange" />
    <small>
      Maximum Combat Points
    </small>
  </label>
  <input
    type="text"
    class="input"
    placeholder="Pokemon or type"
    v-on:input="handleInput"
  />
  <div v-if="loading" class="loader"></div>
  <SuggestionIndex
    v-else-if="!loading && suggestions"
    v-bind:suggestions="suggestions"
    v-bind:input="input"
  />
  <NoResults v-else-if="input" />
</template>

<script>
// eslint-disable-next-line no-unused-vars
import SuggestionIndex from "./components/SuggestionIndex";
import NoResults from "./components/NoResults";

const URL_PATH =
  "https://gist.githubusercontent.com/bar0191/fae6084225b608f25e98b733864a102b/raw/dea83ea9cf4a8a6022bfc89a8ae8df5ab05b6dcc/pokemon.json";

export default {
  name: "App",
  components: {
    SuggestionIndex,
    NoResults,
  },
  data() {
    return {
      loading: false,
      data: null,
      suggestions: null,
      input: "",
      mcp: false,
    };
  },
  methods: {
    async handleInput(e) {
      this.input = e.target.value;
      if (!this.data) await this.fetchData();
      this.suggestions = this.filterSuggestions();
      console.log(this.suggestions);
    },

    handleChange() {
      console.log("change");
      this.mcp = this.mcp ? false : true;
      this.suggestions = this.filterSuggestions();
      console.log(this.suggestions);
    },

    async fetchData() {
      try {
        this.loading = true;

        const response = await fetch(URL_PATH, { method: "GET" });
        if (response.ok) {
          this.data = await response.json();
          this.loading = false;
        } else {
          console.log(response);
        }
      } catch (err) {
        console.log(err);
      }
    },

    filterSuggestions() {
      if (this.input.length < 1) return [];
      let filtered = this.data.filter(
        (i) =>
          i.Name.toLowerCase().includes(this.input.toLowerCase()) ||
          i.Types.some((t) =>
            t.toLowerCase().includes(this.input.toLowerCase())
          )
      );
      return this.mcp
        ? this.sortByMcp(filtered)
        : this.sortSuggestions(filtered);
    },

    sortSuggestions(filtered) {
      return filtered
        .sort((i) => {
          if (i.Name.toLowerCase().includes(this.input.toLowerCase())) {
            return -1;
          }
        })
        .slice(0, 4);
    },

    sortByMcp(filtered) {
      return filtered
        .sort((a, b) => {
          return a.MaxCP > b.MaxCP ? -1 : 1;
        })
        .slice(0, 4);
    },
  },
};
</script>
