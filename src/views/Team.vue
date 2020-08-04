<template>
  <div class="team">
    <v-container>
      <h1 class="display-1">Team</h1>

      <v-row>
        <v-col sm="6" md="4" lg="3" v-for="(person,index) in team" v-bind:key="index">
          <v-card elevation="0" class="text-center ma-3">
            <!-- Image goes here -->
            <v-avatar size="100" class="mt-5 primary">
              <img v-bind:src="person.avatar" />
            </v-avatar>

            <v-card-text>
              <div class="subheading">{{person.name}}</div>
              <div class="grey--text">{{person.role}}</div>
            </v-card-text>

            <v-card-actions>
              <v-btn text color="primary">
                <v-icon small left>mdi-message</v-icon>
                <span>Message</span>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "team",
  data() {
    return {
      team: [
        {
          name: "The Net Ninja",
          role: "Web developer",
          avatar: "/avatar-1.png",
        },
        { name: "Ryu", role: "Graphic designer", avatar: "/avatar-2.png" },
        { name: "Chun Li", role: "Web developer", avatar: "/avatar-3.png" },
        {
          name: "Gouken",
          role: "Social media maverick",
          avatar: "/avatar-4.png",
        },
        { name: "Yoshi", role: "Sales guru", avatar: "/avatar-5.png" },
      ],
    };
  },
};
</script>

<style>
</style>