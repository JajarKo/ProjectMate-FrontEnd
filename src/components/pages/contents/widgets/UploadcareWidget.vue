<script>
import uploadcare from "uploadcare-widget";

export default {
  props: {
    buttonText: {
      type: String,
      required: true,
    },
    buttonClass: {
      type: String,
      required: true,
    },
    publicKey: {
      type: String,
      required: true,
    },
    value: {
      type: String,
      required: false,
    },
    options: {
      type: Object,
      required: false,
    },
  },
  computed: {
    file: function () {
      if (!this.value) return null;
      return uploadcare.fileFrom("uploaded", this.value, {
        publicKey: this.publicKey,
      });
    },
  },
  methods: {
    openDialog: function () {
      const dialog = uploadcare.openDialog(this.file, {
        publicKey: this.publicKey,
        ...this.options,
      });
      dialog.done((file) => {
        file.done((info) => this.$emit("input", info.cdnUrl));
      });
    },
  },
};
</script>

<template>
  <div @click.prevent="openDialog">
    <button type="button" class="btn" :class="buttonClass">
      <i class="bi bi-cloud-upload me-1"></i>
      {{ buttonText }}
    </button>
  </div>
</template>
