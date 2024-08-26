<template>
  <div id="app">
    <h1>File Upload and API Interaction</h1>
    <div class="grid">
      <FileUpload @file-selected="handleFileSelection" />
      <ApiCall :file="selectedFile" @api-response="handleApiResponse" />
      <FilePut :file="selectedFile" :putUrl="putUrl" />
      <AssetPreview :fileId="assetId" />
    </div>
  </div>
</template>

<script>
/* eslint-disable */ 
import FileUpload from './components/FileUpload.vue'
import ApiCall from './components/ApiCall.vue'
import FilePut from './components/FilePut.vue'
import AssetPreview from './components/AssetPreview.vue';

export default {
  name: 'App',
  components: {
    FileUpload,
    ApiCall,
    FilePut,
    AssetPreview
  },
  data() {
    return {
      selectedFile: null,
      putUrl: '',
      assetId: ''
    }
  },
  methods: {
    handleFileSelection(file) {
      this.selectedFile = file
    },
    handleApiResponse({ putUrl, assetId }) {
      this.putUrl = putUrl
      this.assetId = assetId
    }
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
}
.empty-block {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 5px;
}
</style>