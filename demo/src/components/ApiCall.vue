<template>
  <div class="component">
    <h2>Get PUT presigned URL</h2>
    <h4>POST asset-service-url/asset/presigned-url</h4>
    <button @click="callApi" :disabled="!file">Get Presigned URL</button>
    <pre v-if="response">{{ response }}</pre>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'ApiCall',
  props: {
    file: {
      type: File,
      required: true
    }
  },
  data() {
    return {
      response: null
    }
  },
  methods: {
    async callApi() {
      if (!this.file) {
        this.response = 'Error: No file selected'
        return
      }

      const payload = {
        assets: [
          {
            fileName: this.file.name,
            fileSize: this.file.size,
            type: 'centre-license'
          }
        ]
      }

      try {
        const result = await axios.post('https://odin.staging.littlelives.com/asset/presigned-url', payload, {
          headers: {
            'Authorization': `Bearer ${process.env.VUE_APP_API_TOKEN}`,
            'Content-Type': 'application/json'
          }
        });
        this.response = JSON.stringify(result.data, null, 2)
        if (result?.data?.data?.length >= 0) {
          const { url } = result?.data?.data[0].presignedUrl
          const { assetId } = result?.data?.data[0]
          this.$emit('api-response', { putUrl: url, assetId })
        }
      } catch (error) {
        this.response = 'Error: ' + error.message
      }
    },
    getFileType(mimeType) {
      // This is a simple mapping. You might need to expand this based on your FileTypeEnum
      const mimeToEnum = {
        'image/jpeg': 'IMAGE',
        'image/png': 'IMAGE',
        'application/pdf': 'DOCUMENT',
      }
      return mimeToEnum[mimeType] || 'OTHER'
    }
  }
}
</script>
