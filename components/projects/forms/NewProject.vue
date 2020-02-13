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
          v-text="'Add Project'"
        />
        <!-- form fields -->
        <v-container class="pb-0">
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between py-0" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12" class="py-0">
                  <v-text-field
                    prepend-icon="mdi-account-badge-horizontal"
                    placeholder="Project Name"
                    v-model="formData.name"
                  />
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field
                prepend-icon="mdi-text"
                placeholder="Project Description"
                v-model="formData.description"
              />
            </v-col>
          </v-row>
        </v-container>
        <!-- form buttons -->
        <v-card-actions>
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
      description: undefined,
      employees: []
    }
  }),
  methods: {
    resetForm() {
      this.formData = {
        name: undefined,
        description: undefined,
        employees: []
      };
    },
    async submitForm() {
      await this.$axios.$post("/projects", this.formData);
      this.resetForm();
      this.$emit("project-added");
      this.dialog = false;
    }
  }
};
</script>
