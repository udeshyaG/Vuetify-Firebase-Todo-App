<template>
  <div class="projects">
    <v-container>
      <h1 class="display-1 my-5">My Projects</h1>
      <v-expansion-panels>
        <v-expansion-panel v-for="(project,index) in myProjects" :key="index">
          <v-expansion-panel-header>{{project.title}}</v-expansion-panel-header>
          <v-expansion-panel-content>
            <div class="font-weight-bold primary--text mb-3">Due by {{project.due}} 📅</div>
            <div>{{project.content}}</div>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-container>
  </div>
</template>

<script>
import db from "../fb";

export default {
  name: "projects",

  data() {
    return {
      projects: [],
    };
  },

  computed: {
    myProjects() {
      return this.projects.filter((pp) => pp.person === "The Net Ninja");
    },
  },

  created() {
    db.collection("projects").onSnapshot((response) => {
      const changes = response.docChanges();

      changes.forEach((change) => {
        if (change.type === "added") {
          this.projects.push({
            ...change.doc.data(),
            id: change.doc.id,
          });
        }
      });
    });
  },
};
</script>

<style>
</style>