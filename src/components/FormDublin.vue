<template>
    <div id="dublin">
        <h1>Dublin</h1>
        <div v-for="question in filteredQuestion" :key="question.key"></div>
    </div>
</template>

<script>
export default {
    name:"dublin",
    props: {
    questionsData: {
      type: Array,
      required: true
    }
  },
  data() {
    return {};
  },
  methods: {
    regexChecker(question) {
      if (question.rules) {
        if (question.rules.regex.test(question.values)) {
          const q = this.questionsData.filter(
            item => item.key === question.followUpKey
          );
          if (q === 'undefined') {
            q[0].showQuestion = true;
            this.$log.info(q);
          }
        } else {
          const q = this.questionsData.filter(
            item => item.key === question.followUpKey
          );
          if (q === 'undefined') {
            q[0].showQuestion = false;
          }
        }
      }
    }
  },
  computed: {
    filteredQuestion() {
      const pattern = /dublin/;
      var filtered = this.questionsData.filter(function(item) {
        return pattern.test(item.key);
      });
      return filtered;
    }
  }
};
</script>