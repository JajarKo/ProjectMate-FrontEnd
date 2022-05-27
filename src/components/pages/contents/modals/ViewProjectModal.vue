<script>
import UploadcareWidget from "../widgets/UploadcareWidget.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";

export default {
  components: {
    UploadcareWidget,
  },
  props: ["project", "onDeleteProject", "onEditProject"],
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      edit_mode: false,
      project_form: JSON.parse(JSON.stringify(this.project)),
    };
  },
  validations() {
    return {
      project_form: {
        image: { required },
        title: { required },
        description: { required },
        qualification: { required },
        positions: { required },
        company: { required },
        project_contact: { required },
        project_password: { required },
      },
    };
  },
  methods: {
    toggleEdit() {
      this.edit_mode = !this.edit_mode;
      if (this.edit_mode) {
        this.project_form = JSON.parse(JSON.stringify(this.project));
      }
    },
    uploadedProjectFile(cdnUrl) {
      this.project_form.image = cdnUrl;
    },
    submitEditProjectForm() {
      this.onEditProject({ ...this.project_form, id: this.project.id });
      this.v$.$reset();
      this.toggleEdit();
    },
  },
};
</script>

<template>
  <div class="modal-dialog modal-xl" style="cursor: auto">
    <div class="modal-content">
      <form>
        <div class="modal-header">
          <h5 class="modal-title">ดูรายละเอียดโปรเจคงาน</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
          ></button>
        </div>

        <div class="modal-body">
          <div class="container">
            <div class="row">
              <div class="col-lg-6 align-self-center">
                <UploadcareWidget
                  v-if="edit_mode"
                  buttonText="อัพโหลดใหม่"
                  buttonClass="btn-warning"
                  publicKey="c216308c562da69b96a9"
                  @input="uploadedProjectFile"
                />
                <img
                  v-else
                  :src="project.image"
                  class="img-fluid rounded-4"
                  alt="image"
                />
              </div>

              <div class="col-lg-6">
                <div class="mb-3">
                  <label class="col-form-label">ชื่องาน:</label>
                  <input
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{ 'is-invalid': v$.project_form.title.$error }"
                    v-model="project_form.title"
                    @blur="v$.project_form.title.$touch"
                  />
                  <p v-else>{{ project.title }}</p>
                </div>
                <div class="mb-3">
                  <label class="col-form-label">รายละเอียดงาน:</label>
                  <textarea
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{
                      'is-invalid': v$.project_form.description.$error,
                    }"
                    rows="4"
                    v-model="project_form.description"
                    @blur="v$.project_form.description.$touch"
                  />
                  <p v-else>{{ project.description }}</p>
                </div>
                <div class="mb-3">
                  <label class="col-form-label">คุณสมบัติผู้ร่วมงาน:</label>
                  <textarea
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{
                      'is-invalid': v$.project_form.qualification.$error,
                    }"
                    rows="4"
                    v-model="project_form.qualification"
                    @blur="v$.project_form.qualification.$touch"
                  />
                  <p v-else>{{ project.qualification }}</p>
                </div>
                <div class="mb-3">
                  <label class="col-form-label">ตำแหน่งที่รับ:</label>
                  <input
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{ 'is-invalid': v$.project_form.positions.$error }"
                    v-model="project_form.positions"
                    @blur="v$.project_form.positions.$touch"
                  />
                  <p v-else>{{ project.positions }}</p>
                </div>
                <div class="mb-3">
                  <label class="col-form-label">องค์กร:</label>
                  <input
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{ 'is-invalid': v$.project_form.company.$error }"
                    v-model="project_form.company"
                  />
                  <p v-else>{{ project.company }}</p>
                </div>
                <div class="mb-3">
                  <label class="col-form-label">ติดต่อ:</label>
                  <input
                    v-if="edit_mode"
                    type="text"
                    class="form-control"
                    :class="{
                      'is-invalid': v$.project_form.project_contact.$error,
                    }"
                    v-model="project_form.project_contact"
                    @blur="v$.project_form.project_contact.$touch"
                  />
                  <p v-else>{{ project.project_contact }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="modal-footer">
          <div class="text-end">
            <i
              v-if="edit_mode"
              class="bi bi-save h4 text-success mx-2"
              :class="{
                'text-success': !v$.$invalid,
                'text-muted': v$.$invalid,
              }"
              style="cursor: pointer"
              @click.prevent="submitEditProjectForm"
            ></i>
            <i
              class="bi h4 text-warning mx-2"
              :class="{
                'bi-pencil-square': !edit_mode,
                'bi-x-square': edit_mode,
              }"
              style="cursor: pointer"
              @click="toggleEdit"
            ></i>
            <i
              class="bi bi-trash h4 text-danger mx-2"
              style="cursor: pointer"
              @click="onDeleteProject(project.id)"
              data-bs-dismiss="modal"
            ></i>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>
