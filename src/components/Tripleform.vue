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
          rules: { message: "8 characters minimum.", regex: /.{7,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          options: [],
          showQuestion: true
        },
        {
          key: "com.rcs.question.procedural.dateApplied",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "Date",
          values: "",
          showQuestion: false
        },
        {
          key: "com.rcs.question.procedural.locationApplied",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.procedural.dateAdded",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "Date",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.procedural.locationAdded",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.procedural.dateInterview",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "Date",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.procedural.locationInterview",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.surname",
          rules: { message: "8 characters minimum.", regex: /.{7,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          options: [],
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.name",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: false
        },
        {
          key: "com.rcs.question.personal.dateOfBirth",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "Date",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.nationality",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.placeOfBirth",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.maritalStatus",
          rules: { message: "8 characters minimum.", regex: /.{8,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
        },
        {
          key: "com.rcs.question.personal.religion",
          rules: { message: "8 characters minimum.", regex: /.{1,}/ },
          followUpKey: "",
          type: "text",
          values: "",
          showQuestion: true
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
<style scoped></style>
