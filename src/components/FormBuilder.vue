<template>
  <div id="formbuilder" class="container">
    <div>Heloo Form Builder</div>
    <div class="col-lg-12">
      <div class="row  form-row" v-for="(row, index) in rows" :key="index">
        <div class="col-lg-12">
          <div class="row">
            <div class="col-lg-12 row">
              <div class="col-lg-2">
                <select name="" id="" class="form-control" v-model="row.select">
                  <option
                    :value="option.value"
                    v-for="(option, key) in options"
                    :key="key"
                    >{{ option.label }}</option
                  >
                </select>
              </div>
              <div class="col-lg-5">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Question"
                  v-model="row.name"
                />
              </div>
              <div class="col-lg-2">
                <a for=""> Parent Question</a>
              </div>
              <div class="col-lg-2">
                <select
                  name=""
                  id="parentquestion"
                  class="form-control"
                  v-model="row.parentquestion"
                >
                  <option value=""></option>
                  <option
                    :value="key"
                    v-for="(row, key) in getParentQuestion(row)"
                    :key="key"
                    >{{ key }}</option
                  >
                </select>
                <!-- <input
                  type="checkbox"
                  v-model="row.check"
                />&nbsp;&nbsp;Mandatory -->
              </div>
              <div class="col-lg-1">
                <button class="btn btn-danger" @click="deleteRow(index)">
                  X
                </button>
              </div>
            </div>
          </div>
          <div
            class="row"
            v-if="row.select == 3 || row.select == 4 || row.select == 5"
          >
            <div>
              <div
                class="row subrow"
                v-for="(subrow, ind) in row.subrows"
                :key="ind"
              >
                <div class="col-lg-12 row">
                  <div class="col-lg-7">
                    <input
                      type="text"
                      class="form-control"
                      v-model="subrow.answer"
                      placeholder="Answer"
                    />
                  </div>
                  <div class="col-lg-2">
                    <button
                      class="btn btn-danger"
                      @click="deleteSubrow(row, ind)"
                    >
                      X
                    </button>
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-lg-12">
                  <button
                    class="btn btn-success center-block"
                    @click="addSubRow(row)"
                  >
                    Add Answer
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div v-if="showRules(row)">
            <Rules @update="processRule" :wizard-data="row" />
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-lg-12">
          <button type="submit" class="btn btn-info">Submit</button>
          <button type="submit" class="btn btn-success" @click="addRow">
            Add Row
          </button>
        </div>
      </div>
      <!-- <div><pre><code>{{rows}}</code></pre></div> -->
      
    </div>
  </div>
</template>

<script>
import Rules from "./FormRules";
export default {
  name: "formbuilder",
  components: {
    Rules
  },
  data() {
    return {
      options: [
        { label: "Text", value: 1 },
        { label: "Numeric", value: 2 },
        { label: "Check box", value: 3 },
        { label: "Radio button", value: 4 },
        { label: "Drop down", value: 5 },
        { label: "Image", value: 6 },
        { label: "Date", value: 7 }
      ],
      rows: [
        {
          select: 1,
          name: "",
          check: false,
          subrows: [{ answer: "" }],
          parentquestion: "",
          rules: {}
        }
      ]
    };
  },
  methods: {
    addRow: function() {
      this.rows.push({
        select: 1,
        name: "",
        check: false,
        parentquestion: "",
        rules: {},
        subrows: [{ answer: "" }]
      });
    },
    deleteRow: function(index) {
      this.rows.splice(index, 1);
    },
    addSubRow: function(row) {
      row.subrows.push({ answer: "" });
    },
    deleteSubrow: function(row, ind) {
      row.subrows.splice(ind, 1);
    },
    getParentQuestion(row) {
      const filtered = this.rows.filter(function(item) {
        return item.name !== row.name;
      });
      return filtered;
    },
    showRules(row) {
      return row.parentquestion !== "";
    },
    processRule(rules) {
      Object.assign(this.form, rules);
    }
  },
  computed: {}
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
