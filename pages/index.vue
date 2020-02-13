<template>
  <v-content>
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col xs="12" sm="10">
          <div
            :class="{
              headline: $vuetify.breakpoint.smAndDown,
              'display-1': $vuetify.breakpoint.lgAndUp
            }"
            class="text-center pt-5"
          >
            Welcome to the Westpac Employee Database
          </div>
          <div class="headline text-center my-3 pb-9">
            What would you like to search?
          </div>
          <v-row align="center" justify="space-between">
            <v-col
              v-for="item in items"
              :key="item.id"
              sm="3"
              cols="9"
              align="center"
            >
              <nuxt-link :to="item.to" tag="div" exact>
                <div class="circle">
                  <div class="circle-overlay" />
                  <div class="circle-content d-flex flex-column justify-center">
                    <v-icon color="primary" style="font-size: 7vw">{{
                      item.icon
                    }}</v-icon>
                    <p class="font-weight-regular">{{ item.name }}</p>
                  </div>
                </div>
              </nuxt-link>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </v-content>
</template>

<script>
export default {
  data: () => ({
    items: [
      {
        name: "Employees",
        icon: "mdi-account-badge-horizontal",
        to: "/employees"
      },
      {
        name: "Departments",
        icon: "mdi-city-variant",
        to: "/departments"
      },
      {
        name: "Projects",
        icon: "mdi-briefcase-edit",
        to: "/projects"
      }
    ]
  })
};
</script>

<style scoped>
.circle {
  border: solid #b71c1c 10px;
  width: calc(100% + 20px);
  padding-top: 100%;
  border-radius: 100%;
  position: relative;
}

.circle-content {
  position: absolute;
  top: 0px;
  height: 100%;
  width: 100%;
}

.circle:hover {
  cursor: pointer;
}

.circle-overlay {
  background-color: rgba(150, 150, 150);
  opacity: 0;
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  border-radius: 100%;
  transition: 0.5s ease;
  z-index: 1;
}

.circle-overlay:hover {
  opacity: 0.2;
}
</style>
