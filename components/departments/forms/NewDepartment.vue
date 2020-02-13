<template>
  <div>
    <!-- form hidden button -->
    <v-btn
      bottom
      color="primary"
      dark
      fab
      fixed
      right
      @click="dialog = !dialog"
    >
      <v-icon>mdi-plus</v-icon>
    </v-btn>
    <!-- form shown -->
    <v-dialog v-model="dialog" width="800px">
      <v-card>
        <!-- form title -->
        <v-card-title
          style="color:white"
          class="primary"
          v-text="'Add Department'"
        />
        <!-- form fields -->
        <v-container class="pb-0">
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-text-field
                prepend-icon="mdi-city-variant"
                placeholder="Department Name"
                v-model="formData.name"
              />
              <v-text-field
                prepend-icon="mdi-map-marker"
                placeholder="Department Location"
                v-model="formData.location"
              />
            </v-col>
          </v-row>
        </v-container>
        <!-- form buttons -->
        <v-card-actions class="pt-0">
          <v-spacer />
          <v-btn
            text
            color="primary"
            @click="dialog = false"
            v-text="'Cancel'"
          />
          <v-btn text @click="submitForm" v-text="'Submit'" />
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    formData: {
      name: undefined,
      location: undefined,
      projects: [],
      employees: []
    }
  }),
  methods: {
    resetForm() {
      this.formData = {
        name: undefined,
        location: undefined,
        projects: [],
        employees: []
      };
    },
    async submitForm() {
      await this.$axios.$post("/departments", this.formData);
      this.resetForm();
      this.$emit("department-added");
      this.dialog = false;
    }
  }
};
</script>
