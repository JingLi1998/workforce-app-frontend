<template>
  <div>
    <v-speed-dial
      v-model="fab"
      absolute
      right
      bottom
      direction="top"
      transition="scale-transition"
    >
      <template v-slot:activator>
        <v-btn v-model="fab" color="primary" dark fab>
          <v-icon v-if="fab">mdi-close</v-icon>
          <v-icon v-else>mdi-account-circle</v-icon>
        </v-btn>
      </template>
      <v-btn @click="showForm" fab dark small color="blue">
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn @click="add = !add" fab dark small color="indigo">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      <v-btn @click="remove = !remove" fab dark small color="red">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-speed-dial>

    <v-dialog v-model="edit" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white"
          >Edit Project</v-card-title
        >
        <v-container>
          <v-container>
            <v-row class="mx-2">
              <v-col class="align-center justify-space-between" cols="12">
                <v-row align="center" class="mr-0">
                  <v-col cols="12">
                    <v-text-field
                      prepend-icon="mdi-account-badge-horizontal"
                      placeholder="Project Name"
                      v-model="formData.name"
                    />
                  </v-col>
                </v-row>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  prepend-icon="mdi-text"
                  placeholder="Project Description"
                  v-model="formData.description"
                />
              </v-col>
            </v-row>
          </v-container>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="edit = false">Cancel</v-btn>
          <v-btn text @click="submitForm">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="remove" width="800px">
      <v-card>
        <v-card-title class="primary" style="color:white"
          >Delete Project</v-card-title
        >
        <v-container>
          <v-row class="mx-2">Warning. This action cannot be undone.</v-row>
        </v-container>
        <v-card-actions>
          <v-spacer />
          <v-btn text color="primary" @click="remove = false">Cancel</v-btn>
          <v-btn text @click="deleteForm">Delete</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      fab: false,
      add: false,
      edit: false,
      remove: false,
      formData: {
        name: "",
        description: ""
      }
    };
  },
  methods: {
    showForm() {
      this.formData = {
        name: this.project.name,
        description: this.project.description
      };
      this.edit = !this.edit;
    },
    async submitForm() {
      await this.$axios.$put(
        `/projects/${this.$route.params.id}`,
        this.formData
      );
      this.edit = false;
      this.$emit("project-updated");
    },
    async deleteForm() {
      await this.$axios.$delete(`/projects/${this.$route.params.id}`);
      this.remove = false;
      setTimeout(() => {
        this.$router.push("/projects");
      }, 500);
    }
  }
};
</script>
