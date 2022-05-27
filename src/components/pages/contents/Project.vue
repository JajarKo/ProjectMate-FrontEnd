<script>
import ViewProjectModal from "./modals/ViewProjectModal.vue";

export default {
  components: {
    ViewProjectModal,
  },
  props: ["project", "onDeleteProject", "onEditProject", "slide"],
};
</script>

<template>
  <div class="card mx-2" :style="slide ? 'max-width: 250px' : ''">
    <img
      :src="project.image"
      class="card-img-top"
      style="object-fit: cover"
      alt="template"
    />
    <div class="card-body">
      <div class="d-flex align-items-start flex-column bd-highlight mb-3 h-100">
        <h5 class="card-title">{{ project.title }}</h5>
        <p class="card-text overflow-auto">
          {{ project.description }}
        </p>
        <button
          type="button"
          class="btn btn-primary mt-auto"
          data-bs-toggle="modal"
          :data-bs-target="'#projectModal' + project.id"
        >
          ดูเพิ่มเติม
        </button>
      </div>
    </div>
  </div>

  <div class="modal fade" :id="'projectModal' + project.id" tabindex="-1">
    <ViewProjectModal
      :project="project"
      :onDeleteProject="onDeleteProject"
      :onEditProject="onEditProject"
    />
  </div>
</template>

<style lang="scss">
.card {
  height: 400px;
  min-width: 250px;
}

.card-img-top {
  height: 150px;
}

.card-body {
  height: 250px;
}

.card-text {
  font-size: smaller;
}
</style>
