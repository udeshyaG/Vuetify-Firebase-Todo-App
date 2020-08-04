<template>
  <v-dialog v-model="dialog" max-width="500">
    <template v-slot:activator="{ on, attrs }">
      <v-btn class="success" text v-bind="attrs" v-on="on">
        <v-icon left>mdi-folder-plus</v-icon>
        <span>Add New Project</span>
      </v-btn>
    </template>

    <v-card>
      <v-card-title class="headline grey lighten-2">Add a new Project</v-card-title>

      <v-card-text>
        <!-- FORM goes here -->
        <v-form ref="form">
          <v-text-field
            label="Title"
            v-model="title"
            prepend-icon="mdi-file-document-multiple"
            v-bind:rules="inputRules"
          ></v-text-field>
          <v-textarea
            label="Information"
            v-model="content"
            prepend-icon="mdi-pencil"
            v-bind:rules="inputRules"
          ></v-textarea>

          <!-- Date Picker -->
          <v-menu v-model="menu1" :close-on-content-click="false" max-width="290">
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                :value="formattedDate"
                clearable
                prepend-icon="mdi-calendar"
                label="Due Date"
                v-bind:rules="inputRules"
                readonly
                v-bind="attrs"
                v-on="on"
                @click:clear="date = null"
              ></v-text-field>
            </template>
            <v-date-picker v-model="due" @change="menu1 = false"></v-date-picker>
          </v-menu>
        </v-form>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn class="success--text mb-2 mr-4" text @click="submit" v-bind:loading="loadData">Submit</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import moment from "moment";
import db from "../fb";

export default {
  name: "popup",
  data() {
    return {
      dialog: false,
      title: "",
      content: "",
      menu1: false,
      due: null,
      loadData: false,
      inputRules: [
        (value) => value.length >= 3 || "Minimum Length is 3 Characters",
      ],
    };
  },

  methods: {
    async submit() {
      if (this.$refs.form.validate()) {
        this.loadData = true;

        const newProject = {
          title: this.title,
          content: this.content,
          due: moment(this.due).format("Do MMM YYYY"),
          person: "The Net Ninja",
          status: "ongoing",
        };

        const response = await db.collection("projects").add(newProject);

        this.loadData = false;
        this.dialog = false;
        console.log("Added to db", response);

        //Emit an event to Navbar component
        this.$emit("projectAdded");
      }
    },
  },

  computed: {
    formattedDate() {
      return this.due ? moment(this.due).format("Do MMM YYYY") : "";
    },
  },
};
</script>

<style></style>
