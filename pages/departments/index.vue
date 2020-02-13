<template>
  <v-container fluid>
    <app-departments :departments="departments"></app-departments>
    <app-new-department v-on:department-added="refresh"></app-new-department>
  </v-container>
</template>

<script>
import Departments from "../../components/departments/Departments.vue";
import NewDepartment from "../../components/departments/forms/NewDepartment.vue";

export default {
  async asyncData({ $axios }) {
    const data = await $axios.$get("/departments");
    return { departments: data.departments };
  },
  components: {
    appDepartments: Departments,
    appNewDepartment: NewDepartment
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get("/departments");
      this.departments = data.departments;
    }
  }
};
</script>