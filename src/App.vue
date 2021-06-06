<template>
  <v-app>
    <v-main>
     <v-parallax height="300" dark src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg"></v-parallax>
     <v-container>
       <v-row class="justify-center" >
         <v-col cols="10" md="6">
           <v-card>
         <v-list-item-group v-model="clickedTask" >
        <v-list-item :key="task.id" v-for="task in taskList" inactive>
           <template v-slot:default="{ active, }">
            <v-list-item-action>
              <v-checkbox
                :input-value="active"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title>{{task.title}}</v-list-item-title>
            </v-list-item-content>
          </template>
          <v-divider inset></v-divider>
        </v-list-item>
      </v-list-item-group>

       </v-card>
         </v-col>
       </v-row>
     </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
export default {
  name: "App",

  data: () => ({
    taskList: [],
    clickedTask: ''
  }),
  methods: {
    loadTaskList() {
      // GET request using axios with set headers
      const headers = { "Content-Type": "application/json" };
      axios.get("http://generaltodoapi.pythonanywhere.com/api/task-list/", { headers })
    .then(response => this.taskList = response.data)
    .catch(error => {
      this.errorMessage = error.message;
      console.error("There was an error!", error);
    });
    }
  },
  created () {
    axios.defaults.xsrfCookieName = 'csrftoken'
    axios.defaults.xsrfHeaderName = 'X-CSRFTOKEN'
    this.loadTaskList()
  }
};
</script>
