<template>
  <template v-if="suggestions && suggestions.length > 0">
    <ul
      class="suggestions"
      v-bind:key="suggestion.Number"
      v-for="suggestion in suggestions"
    >
      <Suggestion v-bind:suggestion="suggestion" v-bind:input="input" /></ul
  ></template>
  <template v-else-if="input">
    <ul class="suggestions">
      <NoResults /></ul
  ></template>
</template>

<script>
import Suggestion from "./Suggestion";
import NoResults from "./NoResults";
export default {
  name: "SuggestionIndex",
  props: ["suggestions", "input"],
  components: {
    Suggestion,
    NoResults,
  },
  methods: {
    highlight() {},
  },
};
</script>
