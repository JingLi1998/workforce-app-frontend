<template>
  <v-col sm="6" offset="3">
    <v-expand-transition>
      <v-card elevation="1" outlined>
        <v-app-bar flat>
          <v-toolbar-title>Employees</v-toolbar-title>
        </v-app-bar>
        <v-list>
          <!-- <transition-group name="fade"> -->
          <nuxt-link
            :key="employee.id"
            v-for="employee in employees"
            :to="'/employees/' + employee.id"
            tag="div"
          >
            <v-list-item @click="dummy">
              <v-list-item-content>
                <v-list-item-title>
                  {{ employee.salary_id +
                  ': ' +
                  employee.first_name +
                  ' ' +
                  employee.last_name}}
                </v-list-item-title>
                <v-list-item-subtitle v-text="employee.role"></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </nuxt-link>
          <!-- </transition-group> -->
        </v-list>
      </v-card>
    </v-expand-transition>
    <v-row class="my-9" align="start" justify="center">
      <router-link to="/" tag="div">
        <v-btn dark color="red darken-4">Return Home</v-btn>
      </router-link>
    </v-row>
  </v-col>
</template>

<script>
export default {
  props: ["employees"],
  computed: {
    fullName() {
      return (
        this.employee.salary_id +
        ": " +
        this.employee.first_name +
        " " +
        this.employee.last_name
      );
    }
  },
  methods: {
    dummy() {}
  }
};
</script>