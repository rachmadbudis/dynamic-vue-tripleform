<template>
  <div id="formrules">
    <div><h3>Rules</h3></div>
    <div class="col-lg-12 row subrow">
      <div class="col-lg-2"><h4>Show if</h4></div>
      <div class="col-lg-3">
        <h4>Parent Question id: {{wizardData.parentquestion}}</h4>
      </div>
      <div class="col-lg-3">
        <select @change="updateData" class="form-control" name="" id="regex" v-model="rules.regex">
          <option value="/^{expected}$/"> Equal</option>
          <option value="/{expected}/">Contains</option>
        </select>
      </div>
      <div class="col-lg-3">
        <input @input="updateData" class="form-control" type="text" v-model="rules.expected" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "formrules",
  props: {
    wizardData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      rules: {
        regex: "",
        expected: ""
      }
    };
  },
  methods: {
    feedback() {
      this.$emit("update", {
        data: this.wizardData
      });
    },
    updateData(){
        this.wizardData.rules = this.rules;
    }
  }
};
</script>
