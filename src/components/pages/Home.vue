<script>
import Jumbotron from "./contents/Jumbotron.vue";
import Projects from "./Projects.vue";
import Colleagues from "./Colleagues.vue";
import Album from "./contents/Album.vue";

export default {
  components: {
    Jumbotron,
    Projects,
    Colleagues,
    Album,
  },
  props: [
    "projects",
    "colleagues",
    "onAddProject",
    "onAddColleague",
    "onDeleteProject",
    "onDeleteColleague",
    "onEditProject",
    "onEditColleague",
  ],
};
</script>

<template>
  <Jumbotron :onAddProject="onAddProject" :onAddColleague="onAddColleague" />
  <Projects
    :slide="true"
    :projects="projects"
    :onDeleteProject="onDeleteProject"
    :onEditProject="onEditProject"
  />
  <Colleagues
    :slide="true"
    :colleagues="colleagues"
    :onDeleteColleague="onDeleteColleague"
    :onEditColleague="onEditColleague"
  />
  <Album />
</template>
