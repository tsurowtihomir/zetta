<template>
  <div class="form-container">
    <h2>Contact Form</h2>
    <form @submit.prevent="submitForm">
      <div class="form-row">
        <div class="form-group">
          <input
            type="text"
            id="name"
            v-model="formData.name"
            placeholder="Name"
            required
          />
        </div>

        <div class="form-group">
          <input
            type="text"
            id="lastname"
            v-model="formData.lastname"
            placeholder="Last Name"
            required
          />
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <input
            type="email"
            id="email"
            v-model="formData.email"
            placeholder="Email"
            required
          />
        </div>

        <div class="form-group">
          <input
            type="number"
            id="age"
            v-model="formData.age"
            placeholder="Age"
            required
          />
        </div>
      </div>
      <p class="error" v-if="!isAgeValid && (typeof this.formData.age != 'string')">
        Age must be between 0 and 120
      </p>

      <div class="form-row">
        <div class="form-group">
          <select id="color" v-model="formData.favoriteColor" required>
            <option value="" disabled selected>Select Favorite Color</option>
            <option value="red">Red</option>
            <option value="green">Green</option>
            <option value="blue">Blue</option>
            <option value="white">White</option>
            <option value="black">Black</option>
          </select>
        </div>

        <div class="contact-preference">
          <label>Contact Preference:</label>
          <div class="preference-options">
            <div class="preference-option">
              <input
                type="checkbox"
                id="emailPreference"
                v-model="formData.contactPreferences.email"
              />
              <label for="emailPreference">Email</label>
            </div>
            <div class="preference-option">
              <input
                type="checkbox"
                id="phoneCallPreference"
                v-model="formData.contactPreferences.phoneCall"
              />
              <label for="phoneCallPreference">Phone Call</label>
            </div>
            <div class="preference-option">
              <input
                type="checkbox"
                id="smsPreference"
                v-model="formData.contactPreferences.sms"
              />
              <label for="smsPreference">SMS</label>
            </div>
          </div>
        </div>
      </div>
      <p class="error" v-if="isSubmitted == true && isPreferenceSelected == false">
        Select at least one
      </p>

      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: "",
        lastname: "",
        email: "",
        age: "",
        favoriteColor: "",
        contactPreferences: {
          email: false,
          phoneCall: false,
          sms: false,
        },
      },
      isPreferenceSelected: false,
      isSubmitted: false,
    };
  },
  computed: {
    isAgeValid() {
      return this.formData.age < 120 && this.formData.age > 0;
    },
  },
  watch: {
    "formData.contactPreferences": {
      handler(newPreferences) {
        // Check if at least one contact preference is selected
        const selectedPreferences = Object.values(newPreferences);
        this.isPreferenceSelected = selectedPreferences.includes(true);
      },
      deep: true, // Watch nested properties
    },
  },
  methods: {
    submitForm() {
      this.isSubmitted = true;
      // Validate age
      if (this.formData.age <= 0 || this.formData.age > 120) {
        return;
      }

      // At least one contact preference to be selected
      if (this.isPreferenceSelected == false) {
        return;
      }

      // If all validations pass, emit the form data and reset the form
      this.$emit("addData", { ...this.formData });
      this.resetForm();
    },
    resetForm() {
      this.isSubmitted = false;

      // For each key in formData, set default values based on the key:
      //   - If the key is "contactPreferences", create sub-keys with default values as false.
      //   - Otherwise, set the default value as an empty string.
      this.formData = {
        ...Object.fromEntries(
          Object.keys(this.formData).map((key) => [
            key,
            key === "contactPreferences"
              ? Object.fromEntries(
                  Object.keys(this.formData[key]).map((prefKey) => [
                    prefKey,
                    false,
                  ])
                )
              : "",
          ])
        ),
      };
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/FormComponent.scss";
</style>
