<template>
  <t-space direction="vertical">
    <t-space breakLine>
      <t-checkbox v-model="disabled">禁用状态</t-checkbox>
      <t-checkbox v-model="autoUpload">自动上传</t-checkbox>
      <t-checkbox v-model="showThumbnail">显示文件缩略图</t-checkbox>
      <t-checkbox v-model="allowUploadDuplicateFile"> 允许上传同名文件 </t-checkbox>
      <t-checkbox v-model="isBatchUpload"> 整体替换上传 </t-checkbox>
      <t-checkbox v-model="uploadAllFilesInOneRequest"> 多个文件一个请求上传 </t-checkbox>
    </t-space>

    <br />

    <t-upload
      v-model="files"
      action="https://service-bv448zsw-1257786608.gz.apigw.tencentcs.com/api/upload-demo"
      placeholder="支持批量上传文件，文件格式不限，最多只能上传 10 份文件"
      theme="file-flow"
      multiple
      :disabled="disabled"
      :abridge-name="ABRIDGE_NAME"
      :auto-upload="autoUpload"
      :show-thumbnail="showThumbnail"
      :max="10"
      :allow-upload-duplicate-file="allowUploadDuplicateFile"
      :is-batch-upload="isBatchUpload"
      :upload-all-files-in-one-request="uploadAllFilesInOneRequest"
      @dragenter="onDragenter"
      @dragleave="onDragleave"
      @drop="onDrop"
    ></t-upload>
  </t-space>
</template>

<script>
export default {
  name: 'TUploadFileFlow',

  data() {
    return {
      max: 5,
      files: [],
      disabled: false,
      autoUpload: false,
      allowUploadDuplicateFile: false,
      isBatchUpload: false,
      uploadAllFilesInOneRequest: false,
      ABRIDGE_NAME: [10, 7],
      showThumbnail: false,
    };
  },

  methods: {
    onDragenter(p) {
      console.log('dragenter', p);
    },
    onDragleave(p) {
      console.log('dragleave', p);
    },
    onDrop(p) {
      console.log('drop', p);
    },
  },
};
</script>
