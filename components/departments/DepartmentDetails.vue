<template>
  <div>
    <v-row>
      <v-col cols="12" sm="6">
        <v-card>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1" v-text="'Department Employees'" />
            <v-spacer />
            <v-btn @click="showEmployeeForm" icon>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-app-bar>
          <v-list flat>
            <v-list-item-group>
              <v-slide-y-transition v-if="employees.length > 0" class="py-0" group>
                <template v-for="(employee, i) in employees">
                  <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>
                  <v-list-item :key="employee.id">
                    <v-list-item-content>
                      <v-list-item-title
                        v-text="`${ i + 1 }. ${employee.salary_id} - ${ employee.first_name } ${employee.last_name}`"
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
      <v-col cols="12" sm="6">
        <v-card>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1" v-text="'Department Projects'" />
            <v-spacer />
            <v-btn @click="showProjectForm" icon>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-app-bar>
          <v-list>
            <v-list-item-group>
              <v-slide-y-transition v-if="projects.length > 0" class="py-0" group>
                <template v-for="(project, i) in projects">
                  <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>
                  <v-list-item :key="project.id">
                    <v-list-item-content>
                      <v-list-item-title v-text="`${ i + 1 }. ${ project.name }`" />
                    </v-list-item-content>
                  </v-list-item>
                </template>
              </v-slide-y-transition>
              <v-list-item v-else>
                <v-list-item-content>
                  <v-list-item-title>No Projects</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>

    <v-dialog v-model="projectForm" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white">Assign Project</v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12">
                  <v-select
                    v-model="selectedProject"
                    :items="projectList.map(a => a.name)"
                    :rules="[v => !!v || 'A Project is required']"
                    label="Projects"
                    required
                  ></v-select>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="projectForm = false">Cancel</v-btn>
          <v-btn text @click="submitProjectForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="employeeForm" width="800px">
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
          <v-btn text color="primary" @click="employeeForm = false">Cancel</v-btn>
          <v-btn text @click="submitEmployeeForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="projectForm" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white">Assign Project</v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col class="align-center justify-space-between" cols="12">
              <v-row align="center" class="mr-0">
                <v-col cols="12">
                  <v-select
                    v-model="selectedProject"
                    :items="projectList.map(a => a.name)"
                    :rules="[v => !!v || 'A Project is required']"
                    label="Projects"
                    required
                  ></v-select>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="projectForm = false">Cancel</v-btn>
          <v-btn text @click="submitProjectForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["departmentEmployees", "departmentProjects"],
  data() {
    return {
      employeeForm: false,
      projectForm: false,
      employeeList: [],
      projectList: [],
      selectedEmployee: undefined,
      selectedProject: undefined
    };
  },
  computed: {
    employees() {
      return this.departmentEmployees;
    },
    projects() {
      return this.departmentProjects;
    }
  },
  methods: {
    async showEmployeeForm() {
      const data = await this.$axios.$get("/employees");
      console.log(data);
      this.employeeList = data.employees;
      this.employeeForm = !this.employeeForm;
    },
    async submitEmployeeForm() {
      this.selectedEmployee = this.selectedEmployee.replace(/ .*/, "");
      const employeeId = this.employeeList.filter(
        a => a.salary_id == this.selectedEmployee
      )[0].id;
      await this.$axios.$patch(
        `/departments/${this.$route.params.id}/employees/${employeeId}`
      );
      this.$emit("employee-assigned");
      this.employeeForm = !this.employeeForm;
    },
    async showProjectForm() {
      const data = await this.$axios.$get("/projects");
      this.projectList = data.projects;
      this.projectForm = !this.projectForm;
    },
    async submitProjectForm() {
      const projectId = this.projectList.filter(
        a => a.name == this.selectedProject
      )[0].id;
      await this.$axios.$patch(
        `/departments/${this.$route.params.id}/projects/${projectId}`
      );
      this.$emit("project-assigned");
      this.projectForm = !this.projectForm;
    }
  }
};
</script>