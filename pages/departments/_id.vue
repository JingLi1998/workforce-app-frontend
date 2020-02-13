<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-department :department="department"></app-department>
        <app-edit-department :department="department" v-on:department-updated="refresh"></app-edit-department>
      </v-card>
      <app-department-details
        :departmentProjects="projects"
        :departmentEmployees="employees"
        v-on:employee-assigned="refresh"
        v-on:project-assigned="refresh"
      ></app-department-details>
    </v-col>
  </v-container>
</template>

<script>
import Department from "../../components/departments/Department.vue";
import DepartmentDetails from "../../components/departments/DepartmentDetails.vue";
import EditDepartment from "../../components/departments/forms/EditDepartment.vue";

export default {
  async asyncData({ $axios, params }) {
    const [departmentData, projectData, employeeData] = await Promise.all([
      $axios.$get(`/departments/${params.id}`),
      $axios.$get(`/departments/${params.id}/projects`),
      $axios.$get(`/departments/${params.id}/employees`)
    ]);
    return {
      department: departmentData.department,
      projects: projectData.projects,
      employees: employeeData.employees
    };
  },
  components: {
    appDepartment: Department,
    appDepartmentDetails: DepartmentDetails,
    appEditDepartment: EditDepartment
  },
  methods: {
    async refresh() {
      const [departmentData, projectData, employeeData] = await Promise.all([
        this.$axios.$get(`/departments/${this.$route.params.id}`),
        this.$axios.$get(`/departments/${this.$route.params.id}/projects`),
        this.$axios.$get(`/departments/${this.$route.params.id}/employees`)
      ]);
      this.department = departmentData.department;
      this.projects = projectData.projects;
      this.employees = employeeData.employees;
    }
  }
};
</script>