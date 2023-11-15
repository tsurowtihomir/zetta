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
            <button
              @click="showDeleteConfirmation(index)"
              class="delete-button"
            >
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

  <!-- Confirmation Dialog -->
  <div v-if="showConfirmationDialog" class="confirmation-dialog">
    <div class="confirmation-content">
      <p>Are you sure you want to delete this row?</p>
      <div class="confirmation-buttons">
        <button @click="confirmDelete" class="confirm-button">
          Yes, Delete
        </button>
        <button @click="cancelDelete" class="cancel-button">Cancel</button>
      </div>
    </div>
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
      showConfirmationDialog: false,
      rowToDelete: null,
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
    showDeleteConfirmation(index) {
      this.rowToDelete = index;
      this.showConfirmationDialog = true;
    },
    confirmDelete() {
      this.$emit("delete-row", this.rowToDelete);
      this.hideConfirmationDialog();
    },
    cancelDelete() {
      this.hideConfirmationDialog();
    },
    hideConfirmationDialog() {
      this.showConfirmationDialog = false;
      this.rowToDelete = null;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/DataTable.scss";
</style>
