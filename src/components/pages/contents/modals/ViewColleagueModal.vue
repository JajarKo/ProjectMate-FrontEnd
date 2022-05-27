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
        <div class="text-end">
          <i
            v-if="edit_mode"
            class="bi bi-save h4 text-success mx-2"
            :class="{
              'text-success': !v$.$invalid,
              'text-muted': v$.$invalid,
            }"
            style="cursor: pointer"
            @click.prevent="submitEditColleagueForm"
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
            @click="onDeleteColleague(colleague.id)"
            data-bs-dismiss="modal"
          ></i>
        </div>
      </div>
    </div>
  </div>
</template>
