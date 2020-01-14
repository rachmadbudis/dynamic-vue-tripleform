<template>
  <div id="formbuilder" class="container">
    <div>Heloo Form Builder</div>
    <div class="col-lg-12">
      <div class="row  form-row" v-for="(row, index) in questions" :key="index">
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
                  v-model="row.key"
                />
              </div>
              <div class="col-lg-2">
                <a for=""> Parent Question</a>
              </div>
              <div class="col-lg-2">
                <select
                  name=""
                  id="parentKey"
                  class="form-control"
                  v-model="row.parentKey"
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
          <div class="row translation-div" v-if="showTranslation(row)">
            <div class="col-lg-4 row">
              <label class="text-center" for=""> NL Translation</label>
              <input
                class="form-control"
                v-model="row.key_nl"
                type="text"
                placeholder="NL Translations"
              />
            </div>
            <div class="col-lg-4 row">
              <label for=""> AR Translation</label>
              <input
                class="form-control"
                v-model="row.key_ar"
                type="text"
                placeholder="AR Translations"
              />
            </div>
            <div class="col-lg-4 row">
              <label for=""> EN Translation</label>
              <input
                class="form-control"
                v-model="row.key_en"
                type="text"
                placeholder="EN Translations"
              />
            </div>
          </div>
          <div
            class="row"
            v-if="
              row.select === 'Check box' ||
                row.select == 'Radio button' ||
                row.select == 'Drop down'
            "
          >
            <div>
              <div
                class="row subrow"
                v-for="(subrow, ind) in row.options"
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
import { required } from "vuelidate/lib/validators";

export default {
  name: "formbuilder",
  components: {
    Rules
  },
  data() {
    return {
      options: [
        { label: "Text", value: "Text" },
        { label: "Numeric", value: "Numeric" },
        { label: "Check box", value: "Check box" },
        { label: "Radio button", value: "Radio button" },
        { label: "Drop down", value: "Drop down" },
        { label: "Date", value: "Date" }
      ],
      questions: [
        {
          select: "Text",
          key: "",
          options: [{ answer: "" }],
          parentKey: "",
          rules: null,
          key_ar: "",
          key_en: "",
          key_nl: ""
        }
      ]
    };
  },
  methods: {
    addRow: function() {
      this.questions.push({
        select: "Text",
        key: "",
        parentKey: "",
        rules: null,
        options: [{ answer: "" }],
        key_ar: "",
        key_en: "",
        key_nl: ""
      });
    },
    deleteRow: function(index) {
      this.questions.splice(index, 1);
    },
    addSubRow: function(row) {
      row.options.push({ answer: "" });
    },
    deleteSubrow: function(row, ind) {
      row.options.splice(ind, 1);
    },
    getParentQuestion(row) {
      const filtered = this.questions.filter(function(item) {
        return item.key !== row.key;
      });
      return filtered;
    },
    showRules(row) {
      return row.parentKey !== "";
    },
    processRule(rules) {
      Object.assign(this.form, rules);
    },
    showTranslation(row) {
      if (row.key !== "") {
        return true;
      }
    }
  },
  validations: {
    questions: [
      {
        select: {
          required
        },
        key: {
          required
        },
        key_en:{
          required
        }
      }
    ]
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
.text-center {
  text-align: center;
}
.translation-div {
  margin-top: 15px;
  margin-left: 15px;
}
</style>
