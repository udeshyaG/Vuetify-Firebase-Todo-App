<template>
  <div class="dashboard">
    <v-container>
      <h1 class="display-1 my-5">Dashboard</h1>

      <!-- BUTTONS  -->
      <v-row>
        <v-tooltip bottom>
          <template v-slot:activator="{on}">
            <v-btn text color="primary" v-on:click="sortBy('title')" class="mb-4 pl-8" v-on="on">
              <v-icon left>mdi-folder</v-icon>
              <span class="caption">By Project Name</span>
            </v-btn>
          </template>
          <span>Sort project by name</span>
        </v-tooltip>

        <v-tooltip bottom>
          <template v-slot:activator="{on}">
            <v-btn text color="primary" v-on:click="sortBy('person')" class="mb-4 pl-8" v-on="on">
              <v-icon left>mdi-human</v-icon>
              <span class="caption">By Person Name</span>
            </v-btn>
          </template>
          <span>Sort By Name</span>
        </v-tooltip>
      </v-row>

      <v-card flat class="pl-4 mb-2" v-for="(project,index) in projects" v-bind:key="index">
        <v-row v-bind:class="`p-3 project ${project.status}`">
          <v-col sm="12" md="6">
            <div class="caption grey--text">Project Title</div>
            <div>{{project.title}}</div>
          </v-col>

          <v-col sm="4" md="2">
            <div class="caption grey--text">Person</div>
            <div>{{project.person}}</div>
          </v-col>

          <v-col sm="4" md="2">
            <div class="caption grey--text">Due Date</div>
            <div>{{project.due}}</div>
          </v-col>

          <v-col sm="4" md="2">
            <div class="text-right pr-5">
              <v-chip
                small
                v-bind:class="`${project.status} white--text caption mt-3`"
              >{{project.status}}</v-chip>
            </div>
          </v-col>
        </v-row>
      </v-card>
    </v-container>
  </div>
</template>

<script>
import db from "../fb";

export default {
  name: "dashboard",
  data() {
    return {
      projects: [],
    };
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

  methods: {
    sortBy(prop) {
      this.projects.sort((a, b) => (a[prop] < b[prop] ? -1 : 1));
    },
  },
};
</script>

<style  scoped>
.project.completed {
  border-left: 4px solid #3cd1c2 !important;
}

.project.ongoing {
  border-left: 4px solid orange;
}

.project.overdue {
  border-left: 4px solid tomato;
}

.v-chip.completed {
  background: #3cd1c2 !important;
}

.v-chip.ongoing {
  background: orange !important;
}

.v-chip.overdue {
  background: tomato !important;
}
</style>
