<template>
  <div>
    <v-speed-dial v-model="fab" absolute right bottom direction="top" transition="scale-transition">
      <template v-slot:activator>
        <v-btn v-model="fab" color="primary" dark fab>
          <v-icon v-if="fab">mdi-close</v-icon>
          <v-icon v-else>mdi-account-circle</v-icon>
        </v-btn>
      </template>
      <v-btn @click="showForm" fab dark small color="blue">
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn @click="add=!add" fab dark small color="indigo">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      <v-btn @click="remove=!remove" fab dark small color="red">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-speed-dial>

    <v-dialog v-model="edit" width="800px">
      <v-card>
        <v-card-title class="red darken-4" style="color:white">Edit Employee</v-card-title>
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
            <v-col cols="12">
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
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="edit = false">Cancel</v-btn>
          <v-btn text @click="submitForm">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="remove" width="800px">
      <v-card>
        <v-card-title class="red darken-4" style="color:white">Delete Employee</v-card-title>
        <v-container>
          <v-row class="mx-2">Warning. This action cannot be undone.</v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="remove = false">Cancel</v-btn>
          <v-btn text @click="deleteForm">Delete</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["employee"],
  data() {
    return {
      fab: false,
      add: false,
      edit: false,
      remove: false,
      formData: {
        salary_id: "",
        first_name: "",
        last_name: "",
        role: ""
      }
    };
  },
  methods: {
    showForm() {
      this.formData = {
        salary_id: this.employee.salary_id,
        first_name: this.employee.first_name,
        last_name: this.employee.last_name,
        role: this.employee.role
      };
      this.edit = !this.edit;
    },
    async submitForm() {
      await this.$axios.$put(
        `/employees/${this.$route.params.id}`,
        this.formData
      );
      this.edit = false;
      this.$emit("employee-updated");
    },
    async deleteForm() {
      await this.$axios.$delete(`/employees/${this.$route.params.id}`);
      this.remove = false;
      setTimeout(() => {
        this.$router.push("/employees");
      }, 500);
    }
  }
};
</script>
