<script>
import ViewColleagueModal from "./modals/ViewColleagueModal.vue";

export default {
  components: {
    ViewColleagueModal,
  },
  props: ["colleague", "onDeleteColleague", "onEditColleague"],
};
</script>

<template>
  <div class="colleague mx-2 text-center">
    <img
      :src="colleague.image"
      class="colleague-img rounded-circle m-3"
      alt="Cinque Terre"
    />

    <div
      class="colleague-detail d-flex align-items-center flex-column bd-highlight mb-3"
    >
      <h3 class="fw-normal">{{ colleague.name }}</h3>
      <p class="overflow-auto">
        {{ colleague.capability }}
      </p>
      <button
        type="button"
        class="btn btn-secondary mt-auto"
        data-bs-toggle="modal"
        :data-bs-target="'#colleagueModal' + colleague.id"
      >
        ดูเพิ่มเติม
      </button>
    </div>
  </div>

  <div class="modal fade" :id="'colleagueModal' + colleague.id" tabindex="-1">
    <ViewColleagueModal
      :colleague="colleague"
      :onDeleteColleague="onDeleteColleague"
      :onEditColleague="onEditColleague"
    />
  </div>
</template>

<style lang="scss">
.colleague {
  height: 400px;
  min-width: 300px;
  max-width: 300px;
}

.colleague-img {
  width: 128px;
  height: 128px;
  object-fit: cover;
}

.colleague-detail {
  height: 240px;
}
</style>
