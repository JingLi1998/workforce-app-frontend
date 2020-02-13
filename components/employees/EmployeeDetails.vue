<template>
  <div>
    <v-row>
      <v-col sm="4">
        <v-card flat outlined>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1">
              Employee Details
            </v-toolbar-title>
          </v-app-bar>
          <v-list>
            <v-list-item @click="dummy" v-for="n in 3" :key="n.id">
              <v-list-item-content>
                <v-list-item-title>Detail {{ n }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
      <v-col sm="8">
        <v-card flat outlined>
          <v-app-bar color="primary" dark dense flat>
            <v-toolbar-title class="subtitle-1">
              Current Projects
            </v-toolbar-title>
            <v-spacer />
            <v-btn @click="showProjectForm" icon>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-app-bar>
          <v-list flat>
            <v-list-item v-if="projects.length == 0" @click="dummy">
              <v-list-item-content>
                <v-list-item-title>No Projects</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item
              v-else
              @click="dummy"
              v-for="(project, i) in projects"
              :key="project.id"
            >
              <v-list-item-content>
                <v-list-item-title>
                  {{ i + 1 }}. {{ project.name }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white">
          Assign Project
        </v-card-title>
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
          <v-btn text color="primary" @click="dialog = false">Cancel</v-btn>
          <v-btn text @click="submitProjectForm">Assign</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["employeeProjects"],
  data() {
    return {
      dialog: false,
      projectList: [],
      selectedProject: undefined
    };
  },
  computed: {
    projects() {
      return this.employeeProjects;
    }
  },
  methods: {
    dummy() {},
    async showProjectForm() {
      const data = await this.$axios.$get("/projects");
      this.projectList = data.projects;
      this.dialog = !this.dialog;
    },
    async submitProjectForm() {
      const projectId = this.projectList.filter(
        a => a.name == this.selectedProject
      )[0].id;
      await this.$axios.$patch(
        `/employees/${this.$route.params.id}/projects/${projectId}`
      );
      this.$emit("project-assigned");
      this.dialog = !this.dialog;
    }
  }
};
</script>
