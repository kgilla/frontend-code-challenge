<template>
  <li>
    <img v-bind:src="suggestion.img" v-bind:alt="suggestion.Name" />
    <div class="info">
      <h1 v-html="highlight"></h1>
      <template
        v-for="type in suggestion.Types"
        v-bind:key="suggestion.Types.indexOf(type)"
      >
        <span v-bind:class="'type ' + type.toLowerCase()">{{ type }}</span>
      </template>
    </div>
  </li>
</template>

<script>
export default {
  name: "Suggestion",
  props: ["suggestion", "input"],
  methods: {},
  computed: {
    highlight() {
      return this.suggestion.Name.replace(
        new RegExp(this.input, "gi"),
        (match) => {
          return `<span class="hl">${match}</span>`;
        }
      );
    },
  },
  render: {},
};
</script>
