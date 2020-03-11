<template>
  <ol>
    <!-- Create an instance of the todo-item component -->
    <!-- <todo-item v-for="item in groceryList" v-bind:todo="item" v-bind:key="item.id"></todo-item> -->
  </ol>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "ComponentTest",
  data: () => ({
    groceryList: [
      { id: 0, text: "Vegetables" },
      { id: 1, text: "Cheese" },
      { id: 2, text: "Whatever else humans are supposed to eat" }
    ]
  })
});
</script>