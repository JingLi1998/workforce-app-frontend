<template>
  <div>
    <v-row>
      <v-col sm="4">
        <v-card>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1">Project Details</v-toolbar-title>
          </v-app-bar>
          <v-list>
            <v-list-item @click="dummy" v-for="n in 3" :key="n.id">
              <v-list-item-content>
                <v-list-item-title>Detail {{ n }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
      <v-col sm="8">
        <v-card>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1">Current Employees</v-toolbar-title>
            <v-spacer />
            <v-btn @click="showEmployeeForm" icon>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-app-bar>
          <v-list>
            <v-list-item-group>
              <v-slide-y-transition v-if="employees.length > 0" class="py-0" group>
                <template v-for="(employee, i) in employees">
                  <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>
                  <v-list-item :key="employee.id">
                    <v-list-item-content>
                      <v-list-item-title
                        v-text="`${ i + 1 }. ${ employee.salary_id } - ${employee.first_name} ${employee.last_name}`"
                      />
                    </v-list-item-content>
                  </v-list-item>
                </template>
              </v-slide-y-transition>
              <v-list-item v-else>
                <v-list-item-content>
                  <v-list-item-title>No Employees</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white">Assign Employee</v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12">
                  <v-select
                    v-model="selectedEmployee"
                    :items="employeeList.map(a => `${a.salary_id} - ${a.first_name} ${a.last_name}`)"
                    :rules="[v => !!v || 'An Employee is required']"
                    label="Employees"
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
          <v-btn text @click="submitEmployeeForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["projectEmployees"],
  data() {
    return {
      dialog: false,
      employeeList: [],
      selectedEmployee: undefined
    };
  },
  computed: {
    employees() {
      return this.projectEmployees;
    }
  },
  methods: {
    async showEmployeeForm() {
      const data = await this.$axios.$get("/employees");
      this.employeeList = data.employees;
      this.dialog = !this.dialog;
    },
    async submitEmployeeForm() {
      this.selectedEmployee = this.selectedEmployee.replace(/ .*/, "");
      const employeeId = this.employeeList.filter(
        a => a.salary_id == this.selectedEmployee
      )[0].id;
      await this.$axios.$patch(
        `/projects/${this.$route.params.id}/employees/${employeeId}`
      );
      this.$emit("employee-assigned");
      this.dialog = !this.dialog;
    }
  }
};
</script>
