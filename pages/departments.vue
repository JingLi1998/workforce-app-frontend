<template>
  <v-content>
    <div>Departments</div>
    <!-- <transition name="fade" mode="out-in"> -->
    <nuxt-child></nuxt-child>
    <!-- </transition> -->
  </v-content>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
