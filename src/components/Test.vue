/* eslint-disable @typescript-eslint/no-explicit-any */


<template>
  <div>
    <p>Bienvenue {{ fullName }}</p>
    <div>Nous sommes le : {{ now }}</div>
    <div>Le résultat du calcul est : {{ resultatGrosCalcul }}</div>
    <div>{{ ok ? 'YES' : 'NO' }}</div>
    <div>
      <a v-bind:href="url" target="_blank">Un lien avec une url provenant de la data</a>
    </div>
    <span v-once>This will never change: {{ message }}</span>
    <p>{{ message }}</p>
    <v-btn small color="primary" v-on:click="reverseMessage">Reverse Message</v-btn>
    <v-col cols="6" sm="6">
      <v-text-field label="Outlined" single-line outlined v-model="message"></v-text-field>
    </v-col>
    <p v-if="seen">On me voit</p>
    <p v-else>On me voit plus</p>
    <ol>
      <li :key="genUniqueKey(todo)" v-for="todo in todos">{{ todo.text }}</li>
    </ol>
    <p>
      Using v-html directive:
      <span v-html="rawHtml"></span>
    </p>
    <v-btn v-bind:disabled="isButtonDisabled">Button</v-btn>
    <v-form v-on:submit.prevent="onSubmit">
      <v-btn type="submit">Soumettre</v-btn>
    </v-form>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

const UNIQUE_KEY_PROP = "__unique_key_prop__";
const KEY_PREFIX = "__key_prefix__" + Date.now() + "_";
let uid = 0;

const isObject = (val: any) => val !== null && typeof val === "object";

const genUniqueKey = (obj: { [x: string]: any }) => {
  if (isObject(obj)) {
    if (UNIQUE_KEY_PROP in obj) {
      return obj[UNIQUE_KEY_PROP];
    }
    const value = KEY_PREFIX + uid++;
    Object.defineProperty(obj, UNIQUE_KEY_PROP, { value });
    return value;
  }
  return obj;
};

Vue.mixin({
  methods: { genUniqueKey }
});

export default Vue.extend({
  name: "Test",
  data: () => ({
    message: "Ceci est un message",
    seen: true,
    firstName: "Quentin",
    lastName: "Barroin",
    isButtonDisabled: true,
    ok: true,
    url: "http://google.fr",
    todos: [
      { text: "Learn JavaScript" },
      { text: "Learn Vue" },
      { text: "Build something awesome" }
    ],
    rawHtml: "<h2>Du contenu HTML : </h2><ol><li>COUCOU</li></ol>"
    // attributeName: "data-smashId"
  }),
  computed: {
    resultatGrosCalcul: function() {
      return Math.pow(2, 53);
    },
    now: function() {
      return Date.now();
    },
    fullName: {
      // accesseur
      get: function() {
        return this.firstName + " " + this.lastName;
      },
      // mutateur
      set: function(newValue: string) {
        const names = newValue.split(" ");
        this.firstName = names[0];
        this.lastName = names[names.length - 1];
      }
    }
  },
  methods: {
    reverseMessage: function() {
      this.message = this.message
        .split("")
        .reverse()
        .join("");
    },
    onSubmit: function() {
      console.log("soumission du formulaire");
    }
  }
});
</script>
