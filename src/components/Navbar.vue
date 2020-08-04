<template>
  <div>
    <v-snackbar v-model="snackbar" v-bind:timeout="4000" top color="success">
      <v-row align="center">
        <span class="pl-4 white--text">You add a new Project</span>
        <v-spacer></v-spacer>
        <v-btn text v-on:click="snackbar = false">Close</v-btn>
      </v-row>
    </v-snackbar>

    <v-app-bar flat color="white">
      <v-app-bar-nav-icon v-on:click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title class="text-uppercase">
        <span class="font-weight-light grey--text">Todo</span>
        <span class="font-weight-bold">Vue</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <!-- Reference -->
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn text color="primary" v-bind="attrs" v-on="on">
            <v-icon left>mdi-chevron-down</v-icon>
            <span>Menu</span>
          </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="(link, index) in links" :key="index" router v-bind:to="link.route">
            <v-list-item-title>{{ link.text }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>

      <v-btn text class="grey--text text--darken-1">
        <v-icon left>mdi-exit-to-app</v-icon>
        <span>Sign Out</span>
      </v-btn>
    </v-app-bar>

    <!-- Drawer Pops up from left. TEMPORARY must be added -->
    <v-navigation-drawer absolute fixed temporary app v-model="drawer" class="primary">
      <v-row justify="center" class="mt-5">
        <v-avatar size="100">
          <img src="/avatar-1.png" />
        </v-avatar>
      </v-row>

      <v-row justify="center">
        <p class="white--text subheading mt-2">Web Developer</p>
      </v-row>

      <v-row justify="center" class="mb-5">
        <Popup v-on:projectAdded="snackbar = true" />
      </v-row>

      <v-divider></v-divider>

      <v-list>
        <v-list-item v-for="(link,index) in links" v-bind:key="index" router v-bind:to="link.route">
          <v-list-item-action>
            <v-icon class="white--text">{{link.icon}}</v-icon>
          </v-list-item-action>

          <v-list-item-content class="m-4">
            <v-list-item-title class="white--text">{{link.text}}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>

<script>
import Popup from "./Popup";

export default {
  name: "navbar",

  components: {
    Popup,
  },
  data() {
    return {
      drawer: false,
      links: [
        { icon: "mdi-view-dashboard", text: "Dashboard", route: "/" },
        { icon: "mdi-folder", text: "My Projects", route: "/projects" },
        { icon: "mdi-human", text: "Team", route: "/team" },
      ],
      snackbar: false,
    };
  },
};
</script>

<style>
</style>