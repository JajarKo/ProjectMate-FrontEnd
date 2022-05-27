<script>
import UploadcareWidget from "../widgets/UploadcareWidget.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";

export default {
  components: {
    UploadcareWidget,
  },
  props: ["colleague", "onDeleteColleague", "onEditColleague"],
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      edit_mode: false,
      colleague_form: JSON.parse(JSON.stringify(this.colleague)),
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
    uploadResumeButtonText() {
      return this.colleague_form.resume ? "อัพโหลดใหม่" : "อัพโหลดไฟล์";
    },
    uploadResumeButtonClass() {
      return this.colleague_form.resume ? "btn-success" : "btn-warning";
    },
    uploadPortfolioButtonText() {
      return this.colleague_form.resume ? "อัพโหลดใหม่" : "อัพโหลดไฟล์";
    },
    uploadPortfolioButtonClass() {
      return this.colleague_form.portfolio ? "btn-success" : "btn-warning";
    },
  },
  methods: {
    toggleEdit() {
      this.edit_mode = !this.edit_mode;
      if (this.edit_mode) {
        this.colleague_form = JSON.parse(JSON.stringify(this.colleague));
      }
    },
    uploadedColleagueImageFile(cdnUrl) {
      this.colleague_form.image = cdnUrl;
    },
    uploadedColleagueResumeFile(cdnUrl) {
      this.colleague_form.resume = cdnUrl;
    },
    uploadedColleaguePortfolioFile(cdnUrl) {
      this.colleague_form.portfolio = cdnUrl;
    },
    submitEditColleagueForm() {
      this.onEditColleague({ ...this.colleague_form, id: this.colleague.id });
      this.v$.$reset();
      this.toggleEdit();
      this.password = "";
    },
  },
};
</script>

<template>
  <div class="modal-dialog modal-xl">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">ดูรายละเอียดเพื่อนร่วมงาน</h5>
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
                @input="uploadedColleagueImageFile"
              />
              <img
                v-else
                :src="colleague.image"
                class="img-fluid rounded-4"
                alt="image"
              />
            </div>

            <div class="col-lg-6">
              <div class="mb-3">
                <label class="col-form-label">ชื่อ:</label>
                <input
                  v-if="edit_mode"
                  type="text"
                  class="form-control"
                  :class="{ 'is-invalid': v$.colleague_form.name.$error }"
                  v-model="colleague_form.name"
                  @blur="v$.colleague_form.name.$touch"
                />
                <p v-else>{{ colleague.name }}</p>
              </div>
              <div class="mb-3">
                <label class="col-form-label">ความสามารถ:</label>
                <textarea
                  v-if="edit_mode"
                  class="form-control"
                  :class="{ 'is-invalid': v$.colleague_form.capability.$error }"
                  id="capability"
                  rows="4"
                  v-model="colleague_form.capability"
                  @blur="v$.colleague_form.capability.$touch"
                />
                <p v-else>{{ colleague.capability }}</p>
              </div>
              <div class="mb-3">
                <label class="col-form-label">ตำแหน่งที่สนใจ:</label>
                <textarea
                  v-if="edit_mode"
                  class="form-control"
                  :class="{
                    'is-invalid': v$.colleague_form.int_pos.$error,
                  }"
                  id="int_pos"
                  rows="4"
                  v-model="colleague_form.int_pos"
                  @blur="v$.colleague_form.int_pos.$touch"
                />
                <p v-else>{{ colleague.int_pos }}</p>
              </div>
              <div class="mb-3">
                <label class="col-form-label">ติดต่อ:</label>
                <input
                  v-if="edit_mode"
                  type="text"
                  class="form-control"
                  :class="{
                    'is-invalid': v$.colleague_form.colleague_contact.$error,
                  }"
                  v-model="colleague_form.colleague_contact"
                  @blur="v$.colleague_form.colleague_contact.$touch"
                />
                <p v-else>{{ colleague.colleague_contact }}</p>
              </div>

              <div class="row mb-3">
                <div v-if="colleague.resume" class="col">
                  <UploadcareWidget
                    v-if="edit_mode"
                    :buttonText="uploadResumeButtonText"
                    :buttonClass="uploadResumeButtonClass + ' w-100'"
                    publicKey="c216308c562da69b96a9"
                    @input="uploadedColleagueImageFile"
                  />
                  <a
                    v-else
                    :href="colleague.resume"
                    target="_blank"
                    role="button"
                    class="btn btn-secondary w-100"
                  >
                    <i class="bi bi-cloud-download me-1"></i>
                    Resume
                  </a>
                </div>
                <div v-if="colleague.portfolio" class="col">
                  <UploadcareWidget
                    v-if="edit_mode"
                    :buttonText="uploadPortfolioButtonText"
                    :buttonClass="uploadPortfolioButtonClass + ' w-100'"
                    publicKey="c216308c562da69b96a9"
                    @input="uploadedColleaguePortfolioFile"
                  />
                  <a
                    v-else
                    :href="colleague.portfolio"
                    target="_blank"
                    role="button"
                    class="btn btn-secondary w-100"
                  >
                    <i class="bi bi-cloud-download me-1"></i>
                    Portfolio
                  </a>
                </div>
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
            @click="auth ? submitEditColleagueForm() : () => {}"
          >
            <i class="bi bi-check-square"></i>
            บันทึกการแก้ไข
          </button>
          <button
            v-if="edit_mode"
            type="button"
            class="btn btn-danger btn-sm ms-2 mt-2"
            :disabled="!auth"
            @click="auth ? onDeleteColleague(project.id) : () => {}"
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
    </div>
  </div>
</template>
