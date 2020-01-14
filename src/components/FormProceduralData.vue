<template>
  <div id="procedural" class="container">
    <form>
      <div v-for="question in filteredQuestion" :key="question.key">
        <div
          class="row form-row"
          v-if="regexChecker(question)"
          :id="question.key"
        >
          <div class="col-lg-6">
            <h4>{{ question.key }}</h4>
          </div>
          <div class="col-lg-6 " v-if="question.type === 'text'">
            <input class="form-values" type="text" v-model="question.values" />
          </div>
          <div class="col-lg-6" v-else-if="question.type === 'Date'">
            <input class="form-values" type="date" v-model="question.values" />
          </div>
          <div class="col-lg-6" v-else-if="question.type === 'checkbox'">
            <input
              class="form-values"
              type="checkbox"
              v-model="question.values"
            />
          </div>
          <div class="col-lg-5" v-else-if="question.type === 'Drop down'">
            <select name="" class="form-control" v-model="question.values">
              <option value=""></option>
              <option
                :value="option"
                v-for="(option, key) in question.options"
                :key="key"
                >{{ option }}</option
              >
            </select>
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
      if (question.rules.regex !== null) {
        const regex = question.rules.regex;
        var parent = this.filteredQuestion.filter(function(item) {
          return item.key === question.parentKey;
        });
        if (regex.test(parent[0].values)) {
          return true;
        }
        return false;
      } else {
        return true;
      }
      //   if (question.rules) {
      //     if (question.rules.regex.test(question.values)) {
      //       const q = this.questionsData.filter(
      //         item => item.key === question.parentKey
      //       );
      //       if (q === "undefined") {
      //         q[0].showQuestion = true;
      //         this.$log.info(q);
      //       }
      //     } else {
      //       const q = this.questionsData.filter(
      //         item => item.key === question.parentKey
      //       );
      //       if (q === "undefined") {
      //         q[0].showQuestion = false;
      //       }
      //     }
      //   }
    }
  },
  computed: {
    filteredQuestion() {
      const pattern = /procedural/;
      var filtered = this.questionsData.filter(function(item) {
        return pattern.test(item.key);
      });
      return filtered;
    }
  }
};
</script>
<style>
.form-values {
  border: 5px solid #e2e2e2;
  width: inherit;
}
</style>
