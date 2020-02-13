<template>
  <v-img
    height="45vh"
    src="https://images.pexels.com/photos/380769/pexels-photo-380769.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260"
  >
    <v-row align="end" class="fill-height">
      <v-col align-self="start" class="pa-0" cols="12">
        <v-btn
          class="mt-9"
          @click="$router.go(-1)"
          absolute
          small
          dark
          fab
          top
          right
          color="primary"
        >
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
      </v-col>
      <v-col class="py-0">
        <v-list-item color="rgba(0, 0, 0, .4)" dark>
          <v-list-item-content>
            <v-list-item-title class="title" v-text="department.name" />
            <v-list-item-subtitle v-text="department.location" />
          </v-list-item-content>
        </v-list-item>
      </v-col>
    </v-row>
  </v-img>
</template>

<script>
export default { props: ["department"] };
</script>
