<template>
  <v-col sm="6" offset="3">
    <v-card elevation="1" outlined>
      <!-- card title -->
      <v-app-bar flat>
        <v-toolbar-title v-text="'Employees'" />
      </v-app-bar>
      <v-list>
        <v-list-item-group>
          <v-slide-y-transition v-if="employees.length > 0" class="py-0" group>
            <!-- employee list -->
            <template v-for="(employee, i) in employees">
              <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>
              <nuxt-link
                :key="employee.id"
                :to="'/employees/' + employee.id"
                tag="div"
              >
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title v-text="fullName(employee)" />
                    <v-list-item-subtitle v-text="employee.role" />
                  </v-list-item-content>
                </v-list-item>
              </nuxt-link>
            </template>
          </v-slide-y-transition>
          <!-- if no employees -->
          <v-list-item v-else>
            <v-list-item-content>
              <v-list-item-title v-text="'No employees'" />
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
  <!-- search field -->
  <!-- <v-text-field
    flat
    solo-inverted
    hide-details
    prepend-inner-icon="mdi-magnify"
    label="Search"
    class="hidden-sm-and-down"
  />-->
</template>

<script>
export default {
  props: ["employees"],
  methods: {
    dummy() {},
    fullName(employee) {
      return `${employee.salary_id}: ${employee.first_name} ${employee.last_name}`;
    }
  }
};
</script>
