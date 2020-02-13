<template>
  <v-img height="40vh" src="/employee.png">
    <v-row align="end" class="fill-height">
      <v-col align-self="start" class="pa-0" cols="12">
        <v-avatar class="profile" color="grey" size="164" tile>
          <v-img src="/avatar.png" />
        </v-avatar>
        <v-btn
          class="mt-9"
          @click="$router.go(-1)"
          absolute
          dark
          fab
          top
          right
          small
          color="primary"
        >
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
      </v-col>
      <v-col class="py-0">
        <v-list-item color="rgba(0, 0, 0, .4)" dark>
          <v-list-item-content>
            <v-list-item-title class="title" v-text="fullName" />
            <v-list-item-subtitle v-text="role" />
          </v-list-item-content>
        </v-list-item>
      </v-col>
    </v-row>
  </v-img>
</template>

<script>
export default {
  props: ["currentEmployee"],
  computed: {
    employee() {
      return this.currentEmployee;
    },
    fullName() {
      return this.employee.first_name + " " + this.employee.last_name;
    },
    role() {
      console.log(this.employee.department);
      return this.employee.department
        ? `${this.employee.role} | ${this.employee.department.name}`
        : `${this.employee.role} | No Department`;
    }
  }
};
</script>
