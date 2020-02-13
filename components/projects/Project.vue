<template>
  <v-img height="45vh" src="/project.png">
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
            <v-list-item-title class="title" v-text="project.name" />
            <v-list-item-subtitle v-text="project.description" />
          </v-list-item-content>
        </v-list-item>
      </v-col>
    </v-row>
  </v-img>
</template>

<script>
export default { props: ["project"] };
</script>
