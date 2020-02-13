<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-department :department="department"></app-department>
        <app-edit-department :department="department" v-on:department-updated="refresh"></app-edit-department>
      </v-card>
      <app-department-details></app-department-details>
    </v-col>
  </v-container>
</template>

<script>
import Department from "../../components/departments/Department.vue";
import DepartmentDetails from "../../components/departments/DepartmentDetails.vue";
import EditDepartment from "../../components/departments/forms/EditDepartment.vue";

export default {
  async asyncData({ $axios, params }) {
    const data = await $axios.$get(`/departments/${params.id}`);
    return { department: data.department };
  },
  components: {
    appDepartment: Department,
    appDepartmentDetails: DepartmentDetails,
    appEditDepartment: EditDepartment
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get(
        `/departments/${this.$route.params.id}`
      );
      this.department = data.department;
    }
  }
};
</script>