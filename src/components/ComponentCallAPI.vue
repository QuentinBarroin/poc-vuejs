

<script lang="ts">
import Vue from "vue";
import axios from 'axios';

export default Vue.extend({
  name: "ComponentCallAPI",
  el: "#watch-example",
  data: {
    debouncedGetAnswer: (),
    question: "",
    answer:
      "Je ne peux pas vous donner une réponse avant que vous ne posiez une question !"
  },
  watch: {
    // à chaque fois que la question change, cette fonction s'exécutera
    question: function(newQuestion, oldQuestion) {
      this.answer = "J'attends que vous arrêtiez de taper...";
      this.debouncedGetAnswer();
    }
  },
  created: function() {
    // _.debounce est une fonction fournie par lodash pour limiter la fréquence
    // d'exécution d'une opération particulièrement couteuse.
    // Dans ce cas, nous voulons limiter la fréquence d'accès à
    // yesno.wtf/api, en attendant que l'utilisateur ait complètement
    // fini de taper avant de faire la requête ajax. Pour en savoir
    //  plus sur la fonction `_.debounce` (et sa cousine
    // `_.throttle`), visitez : https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500);
  },
  methods: {
    getAnswer: function() {
      if (this.question.indexOf("?") === -1) {
        this.answer =
          "Les questions contiennent généralement un point d'interrogation. ;-)";
        return;
      }
      this.answer = "Je réfléchis...";
      var vm = this;
      axios
        .get("https://yesno.wtf/api")
        .then(function(response) {
          vm.answer = _.capitalize(response.data.answer);
        })
        .catch(function(error) {
          vm.answer = "Erreur ! Impossible d'accéder à l'API." + error;
        });
    }
  }
});
</script>