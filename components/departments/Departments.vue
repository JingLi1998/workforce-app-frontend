<template>
  <v-col sm="6" offset="3">
    <v-card elevation="1" outlined>
      <!-- card title -->
      <v-app-bar flat>
        <v-toolbar-title v-text="'Departments'" />
      </v-app-bar>
      <v-list>
        <v-list-item-group>
          <v-slide-y-transition
            v-if="departments.length > 0"
            class="py-0"
            group
          >
            <!-- department list -->
            <template v-for="(department, i) in departments">
              <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>
              <nuxt-link
                :key="department.id"
                :to="'/departments/' + department.id"
                tag="div"
              >
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title v-text="department.name" />
                    <v-list-item-subtitle v-text="department.location" />
                  </v-list-item-content>
                </v-list-item>
              </nuxt-link>
            </template>
          </v-slide-y-transition>
          <!-- if no departments -->
          <v-list-item v-else>
            <v-list-item-content>
              <v-list-item-title v-text="'No Departments'" />
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-card>
    <!-- home button -->
    <v-row class="my-9" align="start" justify="center">
      <router-link to="/" tag="div">
        <v-btn dark color="primary">Return Home</v-btn>
      </router-link>
    </v-row>
  </v-col>
</template>

<script>
export default { props: ["departments"] };
</script>
