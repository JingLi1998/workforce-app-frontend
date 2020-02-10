<template>
  <v-row>
    <v-col sm="4">
      <v-card>
        <v-app-bar color="primary" dark dense flat>
          <v-toolbar-title class="subtitle-1">Employee Details</v-toolbar-title>
        </v-app-bar>
        <v-list>
          <v-list-item @click="dummy" v-for="n in 3" :key="n.id">
            <v-list-item-content>
              <v-list-item-title>Detail {{ n }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card>
    </v-col>
    <v-col sm="8">
      <v-card>
        <v-app-bar color="primary" dark dense flat>
          <v-toolbar-title class="subtitle-1">Current Projects</v-toolbar-title>
        </v-app-bar>
        <v-list>
          <v-list-item @click="dummy" v-for="n in 3" :key="n.id">
            <v-list-item-content>
              <v-list-item-title>Project {{ n }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>