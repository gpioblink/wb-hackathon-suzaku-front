<template>
  <!-- ref: https://github.com/darenegade/vuetify_filedrop_sample/blob/master/src/components/FileDrop.vue -->
  <div>
    <v-sheet
      id="dropzone"
      ref="dzone"
      tabindex="0"
      title="Click to grap a file from your PC!"
      color="indigo lighten-4"
      width="100%"
      class="pa-2"
      @dragenter.prevent="dragover = true"
      @dragover.prevent="dragover = true"
      @dragleave.prevent="dragover = false"
      @drop.prevent="dropEvent"
      @click="$refs.upload.click()"
      @keypress.enter="$refs.upload.click()"
    >
      <v-row justify="center" class="mx-5 mt-5">
        <v-icon
          v-if="!dragover"
          color="indigo darken-2"
          size="75"
        >mdi-cloud-upload-outline
        </v-icon>
        <v-icon
          v-if="dragover"
          color="indigo darken-2"
          size="75"
        >mdi-book-plus
        </v-icon>
      </v-row>
      <v-row justify="center" class="mx-5 mb-5">
        <span class="title indigo--text text--darken-2">ドラッグ＆ドロップまたはファイルを選択</span>
      </v-row>
    </v-sheet>
    <input
      ref="upload"
      id="fileUpload"
      type="file"
      :multiple="multiple"
      @change="changeEvent"
      :accept="accept"
    />
  </div>
</template>

<style scoped>
  #dropzone {
    cursor: pointer;
  }
  
  #fileUpload {
    display: none;
  }
</style>

<script lang="ts">
import {Component, Emit, Prop, Vue} from "nuxt-property-decorator"
@Component
export default class FileDrop extends Vue {
    @Prop({type: String, default: () => ""})
    accept!: string

    @Prop({type: Boolean, default: () => false})
    multiple!: boolean
    
    // internal properties
    dragover: boolean = false
    changeEvent(e: Event) {
        const target = (e.target as HTMLInputElement)
        if (target.files) {
            this.filesSelected(target.files)
        }
    }

    dropEvent(dragevent: DragEvent) {
        if (dragevent.dataTransfer) {
            this.filesSelected(dragevent.dataTransfer.files)
        }
    }

    /**
     * upload event...
     */
    @Emit('input')
    filesSelected(fileList: FileList) {
        this.dragover = false
        return fileList;
    }
}
</script>