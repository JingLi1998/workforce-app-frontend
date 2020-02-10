<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-employee :employee="employee"></app-employee>
        <app-edit-employee :employee="employee" v-on:employee-updated="refresh"></app-edit-employee>
      </v-card>
      <app-employee-details></app-employee-details>
    </v-col>
  </v-container>
</template>

<script>
import Employee from "../../components/employees/Employee.vue";
import EmployeeDetails from "../../components/employees/EmployeeDetails.vue";
import EditEmployee from "../../components/employees/forms/EditEmployee.vue";

export default {
  async asyncData({ $axios, params }) {
    const data = await $axios.$get(`/employees/${params.id}`);
    return { employee: data.employee };
  },
  components: {
    appEmployee: Employee,
    appEmployeeDetails: EmployeeDetails,
    appEditEmployee: EditEmployee
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get(
        `/employees/${this.$route.params.id}`
      );
      this.employee = data.employee;
    }
  }
};
</script>