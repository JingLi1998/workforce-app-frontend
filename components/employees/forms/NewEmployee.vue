<template>
  <div>
    <v-btn bottom color="red darken-4" dark fab fixed right @click="dialog = !dialog">
      <v-icon>mdi-plus</v-icon>
    </v-btn>
    <v-dialog v-model="dialog" width="800px">
      <v-card>
        <v-card-title style="color: white" class="red darken-4">Add Employee</v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12">
                  <v-text-field
                    prepend-icon="mdi-account-badge-horizontal"
                    placeholder="Salary ID"
                    v-model="formData.salary_id"
                  />
                </v-col>
              </v-row>
              <v-row align="center" class="mr-0">
                <v-col cols="6">
                  <v-text-field
                    prepend-icon="mdi-account"
                    v-model="formData.first_name"
                    placeholder="First Name"
                  />
                </v-col>
                <v-col cols="6">
                  <v-text-field placeholder="Last Name" v-model="formData.last_name" />
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="6">
              <v-text-field
                prepend-icon="mdi-city-variant"
                placeholder="Department"
                v-model="formData.department"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field placeholder="Role" v-model="formData.role" />
            </v-col>
            <!-- <v-col cols="12">
              <v-text-field disabled prepend-icon="mdi-mail" placeholder="Email" />
            </v-col>
            <v-col cols="12">
              <v-text-field
                disabled
                type="tel"
                prepend-icon="mdi-phone"
                placeholder="(000) 000 - 0000"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field disabled prepend-icon="mdi-text" placeholder="Notes" />
            </v-col>-->
          </v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="dialog = false">Cancel</v-btn>
          <v-btn text @click="submitForm">Save</v-btn>
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
      role: undefined
    }
  }),
  methods: {
    resetForm() {
      this.formData = {
        salary_id: undefined,
        first_name: undefined,
        last_name: undefined,
        role: undefined
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
