<template>
  <v-container fluid>
    <v-col sm="6" offset="3">
      <v-card elevation="1" outlined tile>
        <app-project :project="project"></app-project>
        <app-edit-project :project="project" v-on:project-updated="refresh"></app-edit-project>
      </v-card>
      <app-project-details></app-project-details>
    </v-col>
  </v-container>
</template>

<script>
import Project from "../../components/projects/Project.vue";
import ProjectDetails from "../../components/projects/ProjectDetails.vue";
import EditProject from "../../components/projects/forms/EditProject.vue";

export default {
  async asyncData({ $axios, params }) {
    const data = await $axios.$get(`/projects/${params.id}`);
    return { project: data.project };
  },
  components: {
    appProject: Project,
    appProjectDetails: ProjectDetails,
    appEditProject: EditProject
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get(`/projects/${this.$route.params.id}`);
      this.project = data.project;
    }
  }
};
</script>