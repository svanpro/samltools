<template>
  
    <b-row>
      <base-input v-bind="$props" disabled />
      <base-button
        size="sm"
        class="float-right my-4"
        type="primary"
        outline
        @click="doCopy"
      >
        Copy
      </base-button>
    </b-row>
</template>
<script>
export default {
  name: "copy-box",
  components: {},
  props: {
    value: {
      type: String,
      default: "",
      description: "value to be displayed",
    },
     label: {
        type: String,
        description: "Input label (text before input)"
      }
  },
  methods: {
    doCopy() {
      this.$copyText(this.value).then(
        (success) => {
          console.log("Copied", success);
        },
        (error) => {
          throw new Error(error);
        }
      );
    },
  },
};
</script>
<style>
.el-table.table-dark {
  background-color: #172b4d;
  color: #f8f9fe;
}

.el-table.table-dark th,
.el-table.table-dark tr {
  background-color: #172b4d;
}

.el-table.table-dark td,
.el-table.table-dark th.is-leaf {
  border-bottom: none;
}
</style>
