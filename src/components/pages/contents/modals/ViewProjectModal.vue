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
      password: "",
    };
  },
  computed: {
    auth() {
      return this.password == this.project.project_password;
    },
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
      this.password = "";
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
          <input
            v-if="edit_mode"
            type="password"
            class="form-control"
            :class="{
              'is-invalid': password && !auth,
            }"
            v-model="password"
            placeholder="ใส่รหัสเพื่อแก้ไขข้อมูล"
          />
          <div class="align-self-center text-end">
            <button
              v-if="edit_mode"
              type="button"
              class="btn btn-success btn-sm mt-2"
              :disabled="!auth"
              @click="auth ? submitEditProjectForm() : () => {}"
            >
              <i class="bi bi-check-square"></i>
              บันทึกการแก้ไข
            </button>
            <button
              v-if="edit_mode"
              type="button"
              class="btn btn-danger btn-sm ms-2 mt-2"
              :disabled="!auth"
              @click="auth ? onDeleteProject(project.id) : () => {}"
            >
              <i class="bi bi-x-square"></i>
              ลบโปรเจคงาน
            </button>
            <button
              v-if="edit_mode"
              type="button"
              class="btn btn-secondary btn-sm ms-2 mt-2"
              @click="toggleEdit"
            >
              <i class="bi bi-arrow-up-right-square"></i>
              ยกเลิกการแก้ไข
            </button>

            <button
              v-if="!edit_mode"
              type="button"
              class="btn btn-secondary"
              @click="toggleEdit"
            >
              <i class="bi bi-pencil-square me-2 mt-2"></i>
              แก้ไชโปรเจคงาน
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>
