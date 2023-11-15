<template>
  <div v-if="tableData.length > 0" class="content">
    <button @click="exportData" class="export-button">Export</button>
    <table class="styled-table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Last Name</th>
          <th>Email</th>
          <th>Age</th>
          <th>Favorite Color</th>
          <th>Contact Preferences</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in tableData" :key="index">
          <td>{{ data.name }}</td>
          <td>{{ data.lastname }}</td>
          <td>{{ data.email }}</td>
          <td>{{ data.age }}</td>
          <td>{{ data.favoriteColor }}</td>
          <td>{{ getContactPreferences(data.contactPreferences) }}</td>
          <td>
            <button @click="deleteRow(index)" class="delete-button">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div v-else>
    <p class="no-data-message">No data</p>
  </div>

  <div v-if="exportModalVisible" class="modal" @click="closeExportModal">
    <div class="modal-content" @click.stop>
      <span class="close" @click="closeExportModal">&times;</span>
<!--  Using JSON.stringify to serialize the object with indentation of 2 spaces.
      The 'null' argument indicates no custom replacer function is used. -->
      <pre>{{ JSON.stringify(tableData, null, 2) }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    tableData: Array,
  },
  data() {
    return {
      exportModalVisible: false,
    };
  },
  methods: {
    getContactPreferences(preferences) {
      return Object.keys(preferences)
        .filter((preference) => preferences[preference])
        .join(", ");
    },
    deleteRow(index) {
      this.$emit("delete-row", index);
    },
    exportData() {
      this.exportModalVisible = !this.exportModalVisible;
    },
    closeExportModal() {
      this.exportModalVisible = false;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/DataTable.scss";
</style>
