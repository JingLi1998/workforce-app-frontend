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
          class="primary"
          style="color: white"
          v-text="'Add Employee'"
        />
        <!-- form fields -->
        <v-container class="pb-0">
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between py-0" cols="12">
              <v-row align="center" class="mr-0">
                <v-col class="py-0">
                  <v-text-field
                    prepend-icon="mdi-account-badge-horizontal"
                    placeholder="Salary ID"
                    v-model="formData.salary_id"
                  />
                </v-col>
              </v-row>
              <v-row align="center" class="mr-0">
                <v-col cols="6" class="py-0">
                  <v-text-field
                    prepend-icon="mdi-account"
                    v-model="formData.first_name"
                    placeholder="First Name"
                  />
                </v-col>
                <v-col cols="6" class="py-0">
                  <v-text-field
                    placeholder="Last Name"
                    v-model="formData.last_name"
                  />
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="6" class="py-0">
              <v-text-field
                prepend-icon="mdi-city-variant"
                placeholder="Department"
                v-model="formData.department"
                disabled
              />
            </v-col>
            <v-col cols="6" class="py-0">
              <v-text-field placeholder="Role" v-model="formData.role" />
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
      salary_id: undefined,
      first_name: undefined,
      last_name: undefined,
      role: undefined,
      projects: []
    }
  }),
  methods: {
    resetForm() {
      this.formData = {
        salary_id: undefined,
        first_name: undefined,
        last_name: undefined,
        role: undefined,
        projects: []
      };
    },
    async submitForm() {
      await this.$axios.$post("/employees", this.formData);
      this.resetForm();
      this.$emit("employee-added");
      this.dialog = false;
    }
  }
};
</script>
