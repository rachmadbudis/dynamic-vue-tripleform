<template>
  <div>
    <component
      ref="currentStep"
      :is="currentStep"
      :questionsData="questions"
    ></component>

    <div class="progress-bar">
      <div :style="`width: ${progress}%;`"></div>
    </div>
    <div class="buttons">
      <button @click="goBack" v-if="currentStepNumber > 1" class="btn-outlined">
        Back
      </button>
      <button @click="goNext" class="btn">
        {{ isLastStep ? "Complete Order" : "Next" }}
      </button>
    </div>
  </div>
</template>

<script>
import FormProceduralData from "./FormProceduralData";
import FormCustomerData from "./FormCustomerData";
import FormDublin from "./FormDublin";
import FormDocuments from "./FormDocuments";
export default {
  name: "tripleform",
  components: {
    FormProceduralData,
    FormCustomerData,
    FormDublin,
    FormDocuments
  },
  data() {
    return {
      questions: [
        {
          key: "com.rcs.question.procedural.smartflow",
          key_ar:"",
          key_en:"",
          key_nl:"",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.dateApplied",
          rules: { regex: null },
          parentKey: "",
          type: "Date",
          values: "",
          options: [],
          showQuestion: false
        },
        {
          key: "com.rcs.question.procedural.locationApplied",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.dateAdded",
          rules: { regex: /^YES$/ },
          parentKey: "com.rcs.question.procedural.locationApplied",
          type: "Date",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.locationAdded",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.dateInterview",
          rules: { regex: null },
          parentKey: "",
          type: "Date",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.locationInterview",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.procedural.optional",
          rules: { regex: null },
          parentKey: "",
          type: "Drop down",
          values: "",
          options: ["yes", "no"],
        },
        {
          key: "com.rcs.question.procedural.showhide",
          rules: { regex: /^yes$/ },
          parentKey: "com.rcs.question.procedural.optional",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.personal.surname",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.personal.name",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
          showQuestion: false
        },
        {
          key: "com.rcs.question.personal.dateOfBirth",
          rules: { regex: null },
          parentKey: "",
          type: "Date",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.personal.nationality",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.personal.placeOfBirth",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        },
        {
          key: "com.rcs.question.personal.maritalStatus",
          rules: { regex: null },
          parentKey: "",
          type: "Drop down",
          values: "",
          options: ["Single", "Maried", "Widow"],
        },
        {
          key: "com.rcs.question.personal.religion",
          rules: { regex: null },
          parentKey: "",
          type: "text",
          values: "",
          options: [],
        }
      ],

      currentStepNumber: 1,
      canGoNext: false,
      step: [
        "FormProceduralData",
        "FormCustomerData",
        "FormDublin",
        "FormDocuments"
      ]
    };
  },
  methods: {
    goBack() {
      this.currentStepNumber--;
      this.canGoNext = true;
    },
    goNext() {
      this.currentStepNumber++;
    }
  },
  computed: {
    isLastStep() {
      return this.currentStepNumber === this.length;
    },
    wizardinProgress() {
      return this.currentStepNumber <= this.length;
    },
    length() {
      return this.step.length;
    },
    currentStep() {
      return this.step[this.currentStepNumber - 1];
    },
    progress() {
      return (this.currentStepNumber / this.length) * 100;
    }
  }
};
</script>
<style>
.form-row {
  border: 1px solid #e2e2e2;
  margin: 10px;
  padding: 20px;
  background: #f2f2f2;
}
.subrow {
  margin: 5px;
  padding: 5px;
}
</style>
