<template>
  <v-container fluid>
    <app-employees :employees="employees"></app-employees>
    <app-new-employee v-on:employee-added="refresh"></app-new-employee>
  </v-container>
</template>

<script>
import Employees from "../../components/employees/Employees.vue";
import NewEmployee from "../../components/employees/forms/NewEmployee.vue";

export default {
  async asyncData({ $axios }) {
    const data = await $axios.$get("/employees");
    return { employees: data.employees };
  },
  components: {
    appEmployees: Employees,
    appNewEmployee: NewEmployee
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get("/employees");
      this.employees = data.employees;
    }
  }
};
</script>