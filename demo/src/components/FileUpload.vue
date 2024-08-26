<template>
  <div class="component">
    <h2>File Upload</h2>
    <input type="file" @change="onFileSelected" />
    <div v-if="file">
      <p>File Name: {{ file.name }}</p>
      <p>File Size: {{ (file.size / 1024).toFixed(2) }} KB</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FileUpload',
  data() {
    return {
      file: null
    }
  },
  methods: {
    onFileSelected(event) {
      this.file = event.target.files[0]
      this.$emit('file-selected', this.file)
    }
  }
}
</script>