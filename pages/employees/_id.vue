<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-employee :currentEmployee="employee"></app-employee>
        <app-edit-employee
          :currentEmployee="employee"
          v-on:employee-updated="refresh"
          v-on:department-assigned="refresh"
        ></app-edit-employee>
      </v-card>
      <app-employee-details
        :employeeProjects="projects"
        v-on:project-assigned="refresh"
      ></app-employee-details>
    </v-col>
  </v-container>
</template>

<script>
import Employee from "../../components/employees/Employee.vue";
import EmployeeDetails from "../../components/employees/EmployeeDetails.vue";
import EditEmployee from "../../components/employees/forms/EditEmployee.vue";

export default {
  async asyncData({ $axios, params }) {
    const [employeeData, projectData, departmentData] = await Promise.all([
      $axios.$get(`/employees/${params.id}`),
      $axios.$get(`/employees/${params.id}/projects`),
      $axios.$get(`/employees/${params.id}/departments`)
    ]);
    employeeData.employee.department = departmentData.department;
    console.log(employeeData);
    return {
      employee: employeeData.employee,
      projects: projectData.projects
    };
  },
  components: {
    appEmployee: Employee,
    appEmployeeDetails: EmployeeDetails,
    appEditEmployee: EditEmployee
  },
  methods: {
    async refresh() {
      const [employeeData, projectData, departmentData] = await Promise.all([
        this.$axios.$get(`/employees/${this.$route.params.id}`),
        this.$axios.$get(`/employees/${this.$route.params.id}/projects`),
        this.$axios.$get(`/employees/${this.$route.params.id}/departments`)
      ]);
      employeeData.employee.department = departmentData.department;
      this.employee = employeeData.employee;
      this.projects = projectData.projects;
    }
  }
};
</script>
