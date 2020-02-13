<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-project :project="project"></app-project>
        <app-edit-project :project="project" v-on:project-updated="refresh"></app-edit-project>
      </v-card>
      <app-project-details :projectEmployees="employees" v-on:employee-assigned="refresh"></app-project-details>
    </v-col>
  </v-container>
</template>

<script>
import Project from "../../components/projects/Project.vue";
import ProjectDetails from "../../components/projects/ProjectDetails.vue";
import EditProject from "../../components/projects/forms/EditProject.vue";

export default {
  async asyncData({ $axios, params }) {
    const [projectData, employeeData] = await Promise.all([
      $axios.$get(`/projects/${params.id}`),
      $axios.$get(`/projects/${params.id}/employees`)
    ]);
    return { project: projectData.project, employees: employeeData.employees };
  },
  components: {
    appProject: Project,
    appProjectDetails: ProjectDetails,
    appEditProject: EditProject
  },
  methods: {
    async refresh() {
      const [projectData, employeeData] = await Promise.all([
        this.$axios.$get(`/projects/${this.$route.params.id}`),
        this.$axios.$get(`/projects/${this.$route.params.id}/employees`)
      ]);
      this.project = projectData.project;
      this.employees = employeeData.employees;
    }
  }
};
</script>