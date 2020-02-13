<template>
  <v-container fluid>
    <app-projects :projects="projects"></app-projects>
    <app-new-project v-on:project-added="refresh"></app-new-project>
  </v-container>
</template>

<script>
import Projects from "../../components/projects/Projects.vue";
import NewProject from "../../components/projects/forms/NewProject.vue";

export default {
  async asyncData({ $axios }) {
    const data = await $axios.$get("/projects");
    return { projects: data.projects };
  },
  components: {
    appProjects: Projects,
    appNewProject: NewProject
  },
  methods: {
    async refresh() {
      const data = await this.$axios.$get("/projects");
      this.projects = data.projects;
    }
  }
};
</script>