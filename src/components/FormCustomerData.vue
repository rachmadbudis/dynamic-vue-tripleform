<template>
  <div id="procedural">
    <div>Customer Data</div>

    <form>
      <div v-for="question in filteredQuestion" :key="question.key">
        <div class="row" v-if="question.showQuestion" :id="question.key">
          <div class="col-lg-4">
            <h4>{{ question.key }}</h4>
          </div>
          <div class="col-lg-6" v-if="question.type === 'text'">
            <input
              type="text"
              v-model="question.values"
              @blur="regexChecker(question)"
            />
          </div>
          <div class="col-lg-4" v-else-if="question.type === 'Date'">
            <input type="date" v-model="question.values" />
          </div>
          <div class="col-lg-4" v-else-if="question.type === 'checkbox'">
            <input type="checkbox" v-model="question.values" />
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "procedural",
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
          if (q  === 'undefined') {
            q[0].showQuestion = true;
            this.$log.info(q);
          }
        } else {
          const q = this.questionsData.filter(
            item => item.key === question.followUpKey
          );
          if (q  === 'undefined') {
            q[0].showQuestion = false;
          }
        }
      }
    }
  },
  computed: {
    filteredQuestion() {
      const pattern = /personal/;
      var filtered = this.questionsData.filter(function(item) {
        return pattern.test(item.key);
      });
      return filtered;
    }
  }
};
</script>
