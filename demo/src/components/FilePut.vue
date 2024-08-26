<template>
  <div class="component">
    <h2>File Put</h2>
    <button @click="putFile" :disabled="!file || !putUrl">
      Put File to S3
    </button>
    <p v-if="status">{{ status }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'FilePut',
  props: ['file', 'putUrl'],
  data() {
    return {
      status: ''
    }
  },
  methods: {
    async putFile() {
      if (!this.file || !this.putUrl) {
        this.status = 'Please select a file and get a presigned URL first.'
        return
      }
      try {
        await axios.put(this.putUrl, this.file, {
          headers: { 'Content-Type': this.file.type }
        })
        this.status = 'File uploaded successfully!'
      } catch (error) {
        console.log(error?.message)
        this.status = 'Error uploading file: ' + error.message
      }
    }
  }
}
</script>