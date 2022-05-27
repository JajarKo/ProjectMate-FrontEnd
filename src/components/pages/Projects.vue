<script>
import Project from "./contents/Project.vue";

export default {
  components: {
    Project,
  },
  props: {
    projects: {
      type: Array,
    },
    colleagues: {
      type: Array,
    },
    slide: {
      type: Boolean,
      default: false,
    },
    onAddProject: {
      type: Function,
    },
    onAddColleague: {
      type: Function,
    },
    onDeleteProject: {
      type: Function,
    },
    onDeleteColleague: {
      type: Function,
    },
    onEditProject: {
      type: Function,
    },
    onEditColleague: {
      type: Function,
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="pb-3 mb-4 border-bottom" :class="{ 'mt-4': !slide }">
      <a
        href="/"
        class="d-flex align-items-center text-dark text-decoration-none"
      >
        <i class="bi bi-journal-bookmark me-2" style="font-size: 32px"></i>
        <span class="fs-4">โปรเจคงานสำหรับคุณ</span>
      </a>
    </div>
  </div>

  <div v-if="slide" class="container-fluid px-0 mb-4">
    <div
      class="d-flex flex-row flex-nowrap overflow-scroll px-md-5 pb-3"
      style="cursor: grab"
    >
      <Project
        v-for="project in projects"
        :project="project"
        :onDeleteProject="onDeleteProject"
        :onEditProject="onEditProject"
        :slide="slide"
      />
    </div>
  </div>

  <div v-else class="container">
    <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-3">
      <div class="col" v-for="project in projects">
        <Project
          :project="project"
          :onDeleteProject="onDeleteProject"
          :onEditProject="onEditProject"
          :slide="slide"
        />
      </div>
    </div>
  </div>

  <div
    v-if="projects.length == 0"
    class="text-center text-muted"
    style="height: 376px; padding-top: 64px"
  >
    <h1 class="display-1"><i class="bi bi-book"></i></h1>
    <div class="col-lg-6 mx-auto">
      <p class="lead mb-4">ยังไม่มีโปรไฟลที่ถูกสร้าง :(</p>
    </div>
  </div>
</template>
