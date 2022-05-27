<script>
import CreateProjectModal from "./modals/CreateProjectModal.vue";
import CreateColleagueModal from "./modals/CreateColleagueModal.vue";

export default {
  components: {
    CreateProjectModal,
    CreateColleagueModal,
  },
  props: ["onAddProject", "onAddColleague"],
};
</script>

<template>
  <div class="container py-4">
    <div class="options row align-items-md-stretch">
      <div class="col-md-6 mb-4">
        <div class="project-hero d-flex rounded-3 h-100">
          <div
            class="bar-btn d-flex p-4 text-white bg-dark bg-opacity-75 rounded-3 w-100 mt-auto justify-content-between"
          >
            <h3 class="m-2">สร้างโพสต์ตามหาเพื่อนร่วมโปรเจค</h3>
            <button
              type="button"
              class="btn btn-outline-secondary m-2"
              data-bs-toggle="modal"
              data-bs-target="#createProjectModal"
            >
              คลิก
            </button>
          </div>
        </div>
      </div>

      <div class="col-md-6 mb-4">
        <div class="colleague-hero d-flex rounded-3 h-100">
          <div
            class="bar-btn d-flex p-4 bg-light bg-opacity-75 rounded-3 w-100 mt-auto justify-content-between"
          >
            <h3 class="m-2">สร้างโปรไฟล์สำหรับหางาน</h3>
            <button
              type="button"
              class="btn btn-outline-primary m-2"
              data-bs-toggle="modal"
              data-bs-target="#createColleagueModal"
            >
              คลิก
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="modal fade" id="createProjectModal" tabindex="-1">
    <CreateProjectModal :onAddProject="onAddProject" />
  </div>

  <div class="modal fade" id="createColleagueModal" tabindex="-1">
    <CreateColleagueModal :onAddColleague="onAddColleague" />
  </div>
</template>

<style scoped lang="scss">
.options {
  height: 290px;
}

.bar-btn {
  height: 125px;
}

.project-hero {
  background-image: url("@/assets/project.jpg");
  background-position: center;
}

.colleague-hero {
  background-image: url("@/assets/colleague.jpg");
  background-position: center;
}
</style>
