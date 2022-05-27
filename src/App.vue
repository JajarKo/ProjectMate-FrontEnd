<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      projects: [],
      colleagues: [],
      loading: false,
    };
  },
  methods: {
    async getProjects() {
      this.loading = true;

      try {
        let response = await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/project/read.php"
        );
        this.projects = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    async getColleagues() {
      this.loading = true;

      try {
        let response = await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/colleague/read.php"
        );
        this.colleagues = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    async addProject(project) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify(project),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/project/create.php",
          requestOptions
        );

        await this.getProjects();
      } catch (error) {
        console.log(error);
      }
    },
    async addColleague(colleague) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify(colleague),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/colleague/create.php",
          requestOptions
        );

        await this.getColleagues();
      } catch (error) {
        console.log(error);
      }
    },
    async deleteProject(id) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify({ id: id }),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/project/delete.php",
          requestOptions
        );

        await this.getProjects();
      } catch (error) {
        console.log(error);
      }
    },
    async deleteColleague(id) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify({ id: id }),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/colleague/delete.php",
          requestOptions
        );

        await this.getColleagues();
      } catch (error) {
        console.log(error);
      }
    },
    async editProject(project) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify(project),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/project/update.php",
          requestOptions
        );

        await this.getProjects();
      } catch (error) {
        console.log(error);
      }
    },
    async editColleague(colleague) {
      try {
        const requestOptions = {
          method: "POST",
          body: JSON.stringify(colleague),
        };
        await fetch(
          "https://jjwebprog.000webhostapp.com/ProjectMate/colleague/update.php",
          requestOptions
        );

        await this.getColleagues();
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {
    this.getProjects();
    this.getColleagues();
  },
};
</script>

<template>
  <div id="app">
    <Navbar />
    <router-view
      :projects="projects"
      :colleagues="colleagues"
      :onAddProject="addProject"
      :onAddColleague="addColleague"
      :onDeleteProject="deleteProject"
      :onDeleteColleague="deleteColleague"
      :onEditProject="editProject"
      :onEditColleague="editColleague"
    />
    <Footer />
  </div>
</template>

<style lang="scss">
@use "sass:map";

@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+Thai&family=Roboto&display=swap");
$font-family-base: "Noto Sans Thai", "Roboto", sans-serif;

$primary: #150095;
$secondary: #ffd912;

@import "bootstrap/scss/bootstrap";
@import "bootstrap-icons/font/bootstrap-icons";

#app {
  -ms-overflow-style: none;
  scrollbar-width: none;

  ::-webkit-scrollbar {
    display: none;
  }
}
</style>
