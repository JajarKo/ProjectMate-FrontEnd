<script>
import UploadcareWidget from "../widgets/UploadcareWidget.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";

export default {
  components: {
    UploadcareWidget,
  },
  props: ["onAddProject"],
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      project_form: {
        image: "",
        title: "",
        description: "",
        qualification: "",
        positions: "",
        company: "",
        project_contact: "",
        project_password: "",
      },
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
  computed: {
    uploadButtonText() {
      return this.project_form.image ? "อัพโหลดสำเร็จ กดอีกรอบเพื่อเปลี่ยนไฟล์" : "อัพโหลดไฟล์";
    },
    uploadButtonClass() {
      return this.project_form.image ? "btn-success" : "btn-danger";
    },
  },
  methods: {
    uploadedProjectFile(cdnUrl) {
      this.project_form.image = cdnUrl;
    },
    submitProjectForm() {
      this.onAddProject(this.project_form);
      this.v$.$reset();
      this.project_form = {
        image: "",
        title: "",
        description: "",
        qualification: "",
        positions: "",
        company: "",
        project_contact: "",
        project_password: "",
      };
    },
  },
};
</script>

<template>
  <div class="modal-dialog modal-xl">
    <div class="modal-content">
      <form @submit.prevent="submitProjectForm">
        <div class="modal-header">
          <h5 class="modal-title">สร้างโพสต์โปรเจคงานของคุณ</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
          ></button>
        </div>

        <div class="modal-body">
          <div class="row mb-3">
            <label for="image" class="col-sm-3 col-form-label">รูป</label>
            <div class="col-sm-9">
              <UploadcareWidget
                :buttonText="uploadButtonText"
                :buttonClass="uploadButtonClass"
                publicKey="c216308c562da69b96a9"
                @input="uploadedProjectFile"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="title" class="col-sm-3 col-form-label">หัวข้องาน</label>
            <div class="col-sm-9">
              <input
                type="text"
                class="form-control"
                :class="{ 'is-invalid': v$.project_form.title.$error }"
                id="title"
                v-model="project_form.title"
                @blur="v$.project_form.title.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="description" class="col-sm-3 col-form-label"
              >รายละเอียดงาน</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{ 'is-invalid': v$.project_form.description.$error }"
                id="description"
                rows="4"
                v-model="project_form.description"
                @blur="v$.project_form.description.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="qualification" class="col-sm-3 col-form-label"
              >คุณสมบัติผู้ร่วมงาน</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{ 'is-invalid': v$.project_form.qualification.$error }"
                id="qualification"
                rows="4"
                v-model="project_form.qualification"
                @blur="v$.project_form.qualification.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="positions" class="col-sm-3 col-form-label"
              >ตำแหน่งที่รับ</label
            >
            <div class="col-sm-9">
              <input
                type="text"
                class="form-control"
                :class="{ 'is-invalid': v$.project_form.positions.$error }"
                id="positions"
                v-model="project_form.positions"
                @blur="v$.project_form.positions.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="company" class="col-sm-3 col-form-label"
              >ชื่อองค์กร</label
            >
            <div class="col-sm-9">
              <input
                type="text"
                class="form-control"
                :class="{ 'is-invalid': v$.project_form.company.$error }"
                id="company"
                v-model="project_form.company"
                @blur="v$.project_form.company.$touch"
              />
            </div>
          </div>

          <hr />

          <div class="row mb-3">
            <label for="project_contact" class="col-sm-3 col-form-label"
              >วิธีติดต่อ</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{
                  'is-invalid': v$.project_form.project_contact.$error,
                }"
                id="project_contact"
                rows="4"
                v-model="project_form.project_contact"
                @blur="v$.project_form.project_contact.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="project_password" class="col-sm-3 col-form-label"
              >ตั้งรหัสโพสต์</label
            >
            <div class="col-sm-9">
              <input
                type="password"
                class="form-control"
                :class="{
                  'is-invalid': v$.project_form.project_password.$error,
                }"
                id="project_password"
                v-model="project_form.project_password"
                @blur="v$.project_form.project_password.$touch"
              />
            </div>
          </div>
        </div>

        <div class="modal-footer">
          <button
            type="submit"
            class="btn btn-success w-100 m-2"
            :disabled="v$.$invalid"
            data-bs-dismiss="modal"
          >
            <i class="bi bi-file-earmark-plus me-1"></i>
            สร้างโพสต์
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
