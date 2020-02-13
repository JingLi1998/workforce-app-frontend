<template>
  <div>
    <!-- button group -->
    <v-speed-dial
      v-model="fab"
      absolute
      right
      bottom
      small
      direction="top"
      transition="scale-transition"
    >
      <template v-slot:activator>
        <v-btn v-model="fab" color="primary" dark small fab>
          <v-icon v-if="fab">mdi-close</v-icon>
          <v-icon v-else>mdi-account-circle</v-icon>
        </v-btn>
      </template>
      <v-btn @click="showEditForm" fab dark x-small color="blue">
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn @click="showDepartmentForm" fab dark x-small color="indigo">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      <v-btn @click="remove = !remove" fab dark x-small color="red">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-speed-dial>

    <!-- edit form -->
    <v-dialog v-model="edit" width="800px">
      <v-card>
        <v-card-title
          class="primary"
          style="color:white"
          v-text="'Edit Employee'"
        />
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
        <v-card-actions class="pt-0">
          <v-spacer />
          <v-btn text color="primary" @click="edit = false" v-text="'Cancel'" />
          <v-btn text @click="submitEditForm" v-text="'Save'" />
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- add form -->
    <v-dialog v-model="add" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white">
          Assign Project
        </v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12">
                  <v-select
                    v-model="selectedDepartment"
                    :items="departmentList.map(a => a.name)"
                    :rules="[v => !!v || 'A Department is required']"
                    label="Departments"
                    required
                  ></v-select>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="dialog = false">Cancel</v-btn>
          <v-btn text @click="submitDepartmentForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- delete form -->
    <v-dialog v-model="remove" width="800px">
      <v-card>
        <v-card-title
          class="primary"
          style="color:white"
          v-text="'Delete Employee'"
        />
        <v-container class="pb-0">
          <v-row
            class="mx-2"
            v-text="'Warning. This action cannot be undone.'"
          />
        </v-container>
        <v-card-actions class="pt-0">
          <v-spacer />
          <v-btn text color="primary" @click="remove = false">Cancel</v-btn>
          <v-btn text @click="submitDeleteForm">Delete</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["currentEmployee"],
  data() {
    return {
      fab: false,
      add: false,
      edit: false,
      remove: false,
      departmentList: [],
      selectedDepartment: undefined,
      formData: {
        salary_id: "",
        first_name: "",
        last_name: "",
        role: ""
      }
    };
  },
  computed: {
    employee() {
      return this.currentEmployee;
    }
  },
  methods: {
    showEditForm() {
      this.formData = {
        salary_id: this.employee.salary_id,
        first_name: this.employee.first_name,
        last_name: this.employee.last_name,
        role: this.employee.role
      };
      this.edit = !this.edit;
    },
    async submitEditForm() {
      await this.$axios.$put(
        `/employees/${this.$route.params.id}`,
        this.formData
      );
      this.edit = false;
      this.$emit("employee-updated");
    },
    async submitDeleteForm() {
      await this.$axios.$delete(`/employees/${this.$route.params.id}`);
      this.remove = false;
      setTimeout(() => {
        this.$router.push("/employees");
      }, 500);
    },
    async showDepartmentForm() {
      const data = await this.$axios.$get("/departments");
      this.departmentList = data.departments;
      this.add = !this.add;
    },
    async submitDepartmentForm() {
      const departmentId = this.departmentList.filter(
        a => a.name == this.selectedDepartment
      )[0].id;
      await this.$axios.$patch(
        `/employees/${this.$route.params.id}/departments/${departmentId}`
      );
      this.$emit("department-assigned");
      this.add = !this.add;
    }
  }
};
</script>
