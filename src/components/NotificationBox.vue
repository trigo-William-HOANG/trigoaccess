<template>
    <div class="preview-box">
      <div v-for="(item, index) in items" :key="index" class="preview-row">
        <span class="label">{{ item.label }} | </span>
        <span class="icon">{{ item.icon }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PreviewBox',
    props: {
      items: {
        type: Array,
        required: true,
      }
    }
  }
  </script>
  
  <style scoped>
  .preview-box {
    border: 1px solid #ddd;
    padding: 16px;
    border-radius: 8px;
    width: 400px;
    background-color: #fafafa;
  }
  .preview-row {
    display: flex;
    align-items: center;
    padding: 8px 0;
  }
  .icon {
    font-size: 20px;
    margin-right: 8px;
  }
  .label {
    font-size: 16px;
  }
  </style>
  