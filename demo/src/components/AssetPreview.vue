<template>
  <div class="component">
    <h2>Asset Preview & Get Presigned URL to View</h2>
    <h4>GET asset-service-url/asset/preview/:asset-id</h4>
    <button @click="getPreviewUrl" :disabled="!fileId">Get Preview URL</button>
    <div v-if="response">
      <h3>Response:</h3>
      <pre>{{ JSON.stringify(response, null, 2) }}</pre>
    </div>
    <div v-if="previewUrl">
      <p>Preview URL:</p>
      <a :href="previewUrl" target="_blank">{{ previewUrl }}</a>
    </div>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'AssetPreview',
  props: {
    fileId: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      previewUrl: '',
      error: '',
      response: null
    }
  },
  methods: {
    async getPreviewUrl() {
      if (!this.fileId) {
        this.error = 'No file ID available'
        return
      }

      try {
        const response = await axios.get(`${process.env.VUE_APP_ODIN_SERVICE_URL}/asset/preview/${this.fileId}`, {
          headers: {
            'Authorization': `Bearer ${process.env.VUE_APP_SV_BEARER_TOKEN}`
          }
        })
        this.response = response.data
        this.previewUrl = response.data.data.url
        this.error = ''
      } catch (error) {
        this.error = 'Error getting preview URL: ' + error.message
        this.previewUrl = ''
        this.response = null
      }
    }
  }
}
</script>