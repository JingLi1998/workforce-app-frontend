<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" :clipped="$vuetify.breakpoint.lgAndUp" fixed app>
      <v-list dense>
        <nuxt-link
          tag="div"
          :key="item.text"
          v-for="item in items"
          :to="item.to"
          style="cursor:pointer"
        >
          <v-list-item @click="dummy">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </nuxt-link>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="$vuetify.breakpoint.lgAndUp" app color="primary" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <span class="hidden-sm-and-down font-weight-bold">Westpac Employees</span>
      </v-toolbar-title>
      <!-- <v-text-field
        flat
        solo-inverted
        hide-details
        prepend-inner-icon="mdi-magnify"
        label="Search"
        class="hidden-sm-and-down"
      />-->
      <v-spacer />
      <nuxt-link to="/">
        <v-btn icon>
          <v-icon>mdi-home</v-icon>
        </v-btn>
      </nuxt-link>
      <v-btn icon>
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn icon large>
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.png')" alt="Vuetify" />
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <nuxt />
    <!-- <v-footer fixed app> -->
    <!-- <span>&copy; {{ new Date().getFullYear() }}</span> -->
    <!-- </v-footer> -->
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          icon: "mdi-home",
          text: "Home",
          to: "/"
        },
        {
          icon: "mdi-account-badge-horizontal",
          text: "Employees",
          to: "/employees"
        },
        {
          icon: "mdi-city-variant",
          text: "Departments",
          to: "/departments"
        },
        {
          icon: "mdi-briefcase-edit",
          text: "Projects",
          to: "/projects"
        }
      ],
      drawer: false
    };
  },
  methods: {
    dummy() {}
  }
};
</script>
