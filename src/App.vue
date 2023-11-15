<template>
  <div id="app">
    <FormComponent @addData="addDataToTable" />
    <DataTable :tableData="dataTable" @delete-row="deleteRow" />
  </div>
</template>

<script>
import FormComponent from "./components/FormComponent.vue";
import DataTable from "./components/DataTable.vue";

export default {
  components: {
    FormComponent,
    DataTable,
  },
  data() {
    return {
      dataTable: [],
    };
  },
  methods: {
    addDataToTable(data) {
      this.dataTable.push(data);
    },
    deleteRow(index) {
      this.dataTable.splice(index, 1);
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
