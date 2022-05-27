<script>
import UploadcareWidget from "../widgets/UploadcareWidget.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";

export default {
  components: {
    UploadcareWidget,
  },
  props: ["onAddColleague"],
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      colleague_form: {
        image: "",
        name: "",
        capability: "",
        int_pos: "",
        resume: "",
        portfolio: "",
        colleague_contact: "",
        colleague_password: "",
      },
    };
  },
  validations() {
    return {
      colleague_form: {
        image: { required },
        name: { required },
        capability: { required },
        int_pos: { required },
        resume: {},
        portfolio: {},
        colleague_contact: { required },
        colleague_password: { required },
      },
    };
  },
  computed: {
    uploadImageButtonText() {
      return this.colleague_form.image ? "อัพโหลดสำเร็จ กดอีกรอบเพื่อเปลี่ยนไฟล์" : "อัพโหลดไฟล์";
    },
    uploadImageButtonClass() {
      return this.colleague_form.image ? "btn-success" : "btn-danger";
    },
    uploadResumeButtonText() {
      return this.colleague_form.resume ? "อัพโหลดสำเร็จ กดอีกรอบเพื่อเปลี่ยนไฟล์" : "อัพโหลดไฟล์";
    },
    uploadResumeButtonClass() {
      return this.colleague_form.resume ? "btn-success" : "btn-warning";
    },
    uploadPortfolioButtonText() {
      return this.colleague_form.portfolio ? "อัพโหลดสำเร็จ กดอีกรอบเพื่อเปลี่ยนไฟล์" : "อัพโหลดไฟล์";
    },
    uploadPortfolioButtonClass() {
      return this.colleague_form.portfolio ? "btn-success" : "btn-warning";
    },
  },
  methods: {
    uploadedColleagueImageFile(cdnUrl) {
      this.colleague_form.image = cdnUrl;
    },
    uploadedColleagueResumeFile(cdnUrl) {
      this.colleague_form.resume = cdnUrl;
    },
    uploadedColleaguePortfolioFile(cdnUrl) {
      this.colleague_form.portfolio = cdnUrl;
    },
    submitColleagueForm() {
      this.onAddColleague(this.colleague_form);
      this.v$.$reset();
      this.colleague_form = {
        image: "",
        name: "",
        capability: "",
        int_pos: "",
        resume: "",
        portfolio: "",
        colleague_contact: "",
        colleague_password: "",
      };
    },
  },
};
</script>

<template>
  <div class="modal-dialog modal-xl">
    <div class="modal-content">
      <form @submit.prevent="submitColleagueForm">
        <div class="modal-header">
          <h5 class="modal-title">สร้างโพสต์โปรไฟล์ของคุณ</h5>
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
                :buttonText="uploadImageButtonText"
                :buttonClass="uploadImageButtonClass"
                publicKey="c216308c562da69b96a9"
                @input="uploadedColleagueImageFile"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="name" class="col-sm-3 col-form-label"
              >ชื่อ-นามสกุล</label
            >
            <div class="col-sm-9">
              <input
                type="text"
                class="form-control"
                :class="{ 'is-invalid': v$.colleague_form.name.$error }"
                id="name"
                v-model="colleague_form.name"
                @blur="v$.colleague_form.name.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="capability" class="col-sm-3 col-form-label"
              >ความสามารถ</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{ 'is-invalid': v$.colleague_form.capability.$error }"
                id="capability"
                rows="4"
                v-model="colleague_form.capability"
                @blur="v$.colleague_form.capability.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="int_pos" class="col-sm-3 col-form-label"
              >ตำแหน่งที่สนใจ</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{
                  'is-invalid': v$.colleague_form.int_pos.$error,
                }"
                id="int_pos"
                rows="4"
                v-model="colleague_form.int_pos"
                @blur="v$.colleague_form.int_pos.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="resume" class="col-sm-3 col-form-label">Resume</label>
            <div class="col-sm-9">
              <UploadcareWidget
                :buttonText="uploadResumeButtonText"
                :buttonClass="uploadResumeButtonClass"
                publicKey="c216308c562da69b96a9"
                @input="uploadedColleagueResumeFile"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="portfolio" class="col-sm-3 col-form-label"
              >Portfolio</label
            >
            <div class="col-sm-9">
              <UploadcareWidget
                :buttonText="uploadPortfolioButtonText"
                :buttonClass="uploadPortfolioButtonClass"
                publicKey="c216308c562da69b96a9"
                @input="uploadedColleaguePortfolioFile"
              />
            </div>
          </div>

          <hr />

          <div class="row mb-3">
            <label for="colleague_contact" class="col-sm-3 col-form-label"
              >วิธีติดต่อ</label
            >
            <div class="col-sm-9">
              <textarea
                class="form-control"
                :class="{
                  'is-invalid': v$.colleague_form.colleague_contact.$error,
                }"
                id="colleague_contact"
                rows="4"
                v-model="colleague_form.colleague_contact"
                @blur="v$.colleague_form.colleague_contact.$touch"
              />
            </div>
          </div>

          <div class="row mb-3">
            <label for="colleague_password" class="col-sm-3 col-form-label"
              >ตั้งรหัสโพสต์</label
            >
            <div class="col-sm-9">
              <input
                type="password"
                class="form-control"
                :class="{
                  'is-invalid': v$.colleague_form.colleague_password.$error,
                }"
                id="colleague_password"
                v-model="colleague_form.colleague_password"
                @blur="v$.colleague_form.colleague_password.$touch"
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
            <i class="bi bi-person-plus me-1"></i>
            สร้างโปรไฟล์
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
