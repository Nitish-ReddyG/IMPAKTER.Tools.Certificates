<template>
  <b-container>
    <b-row class="main_row">
      <b-col cols="8">
        <b-form-group
            :label="titleShort + title"
            v-slot="{ ariaDescribedby }"
            label-size="lg"
        >
          <b-form-checkbox
              class="flex_and_start"
              v-model="allSelected"
              :indeterminate.sync="indeterminate"
              @change="toggleAll"
          >
            <b>{{ allSelected ? "Un-select All" : "Select All" }}</b>
          </b-form-checkbox>
          <b-form-checkbox-group
              id="checkbox-group-1"
              v-model="selected"
              :options="industries"
              :aria-describedby="ariaDescribedby"
              name="flavour-1"
              stacked
          ></b-form-checkbox-group>
        </b-form-group
        >
      </b-col>

      <b-col></b-col>
    </b-row>
    <b-row class="buttons_row">
      <b-button class="btn" @click="back">Back</b-button>
      <b-button class="btn" variant="primary" @click="next">Next</b-button>
    </b-row>
  </b-container>
</template>

<script>
import IndustryDisplayMixin from "../../../mixins/IndustryDisplayMixin";

export default {
  name: "IndustrySelector",
  data() {
    return {
      allSelected: false,
      titleShort:
          "Please select all the United Nations Sustainable Development Goals (SDGs) applicable to this ",
      indeterminate: true,
    };
  },
  props: {
    title: String,
    selected: [],
  },
  methods: {
    toggleAll(checked) {
      this.selected = checked
          ? this.industries.map((x) => {
            return x.value;
          })
          : [];
    },
    async next() {
      this.selected.sort();
      this.$emit("next", this.selected);
    },
    back() {
      this.$router.go(-1);
    },
  },
  mixins: [IndustryDisplayMixin],
  components: {},
};
</script>

<style scoped>
* {
  font-family: "Montserrat";
  text-align: left;
}

.mainDiv {
  margin-top: -150px;
}

.main_row {
  margin-top: 100px;
}

#checkbox-group-1 {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left !important;
}

.btn {
  color: black;
  border: 2px solid #989898;
  background: white;
}
</style>
