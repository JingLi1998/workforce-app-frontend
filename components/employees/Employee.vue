<template>
  <v-img height="100%" src="https://cdn.vuetifyjs.com/images/cards/server-room.jpg">
    <v-row align="end" class="fill-height">
      <v-col align-self="start" class="pa-0" cols="12">
        <v-avatar class="profile" color="grey" size="164" tile>
          <v-img src="https://cdn.vuetifyjs.com/images/profiles/marcus.jpg"></v-img>
        </v-avatar>
        <v-btn class="mt-9" @click="$router.go(-1)" absolute dark fab top right color="primary">
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
      </v-col>
      <v-col class="py-0">
        <v-list-item color="rgba(0, 0, 0, .4)" dark>
          <v-list-item-content>
            <v-list-item-title class="title">{{ fullName }}</v-list-item-title>
            <!-- <v-list-item-subtitle>{{ department }}</v-list-item-subtitle> -->
            <v-list-item-subtitle>{{ role }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-col>
    </v-row>
  </v-img>
</template>

<script>
export default {
  props: ["employee"],
  // components: {
  //   "employee-edit": EmployeeEdit
  // },
  // data: () => ({
  //   employee: {
  //     salary_id: "",
  //     first_name: "",
  //     last_name: "",
  //     role: "",
  //     department: ""
  //   }
  // }),
  computed: {
    fullName() {
      return this.employee.first_name + " " + this.employee.last_name;
    },
    role() {
      return this.employee.role + " | " + this.employee.department;
    }
  },
  // created() {
  //   this.getEmployee();
  //   eventBus.$on("employeeEditted", formData => {
  //     this.submitForm(formData);
  //   });
  //   eventBus.$once("employeeDeleted", () => {
  //     setTimeout(() => {
  //       this.deleteEmployee();
  //     }, 1000);
  //   });
  //   // this.$forceUpdate();
  // },
  methods: {
    dummy() {}
    // getEmployee() {
    //   axios
    //     .get(`http://localhost:3000/api/employees/${this.$route.params.id}`)
    //     .then(res => {
    //       this.employee = res.data.employee;
    //       eventBus.reloadEmployee(res.data.employee);
    //     })
    //     .catch(err => console.log(err));
    // },
    // submitForm(formData) {
    //   axios
    //     .put(
    //       `http://localhost:3000/api/employees/${this.$route.params.id}`,
    //       formData
    //     )
    //     .then(() => {
    //       this.getEmployee();
    //     })
    //     .catch(err => console.log(err));
    // },
    // deleteEmployee() {
    //   axios
    //     .delete(`http://localhost:3000/api/employees/${this.$route.params.id}`)
    //     .then(() => {
    //       setTimeout(() => {
    //         this.$router.replace("/employees");
    //       }, 500);
    //     })
    //     .catch(err => console.log(err));
    // }
  }
};
</script>
