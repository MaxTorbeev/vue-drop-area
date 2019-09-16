<template>
  <div>
    <div class="drop-area form_element" ref="fileForm" @click.prevent="openFileDialog">
      <p class="form_label">{{ label }}</p>
      <div class="document_drag_zone">
        <div class="drop-files">{{ message }}</div>
        <div class="or">или</div>
        <div class="add_file">
          <input id="files-form-file" type="file" @change="handleFileUpload" ref="fileInput" multiple>
          <a href="#" >Выберите файл</a>
        </div>
      </div>
    </div>
    <div class="added_file" v-if="files.length">
      <template v-for="(file, index) in files">
        <p class="added_file_item mt-2" @click.prevent="removeFile(index)" :key="index">
          {{ file.name }}
        </p>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DropArea',
  props: {
    label: {
      default: 'Документы',
      type: String
    },
    message: {
      default: 'Перетащите файлы сюда',
      type: String
    }
  },
  data(){
    return{
      files: [],
      abilityDragAndDrop: false,
    }
  },
  mounted(){
    this.init();
  },
  methods: {
    init(){
      this.abilityDragAndDrop = this.hasAbilityDragAndDrop();

      if( this.abilityDragAndDrop ){
        ['drag', 'dragstart', 'dragend', 'dragover', 'dragenter', 'dragleave', 'drop'].forEach( function( evt ) {
          this.$refs.fileForm.addEventListener(evt, function(e){
            e.preventDefault();
            e.stopPropagation();
          }.bind(this), false);
        }.bind(this));

        this.$refs.fileForm.addEventListener('drop', function(e){
          for( let i = 0; i < e.dataTransfer.files.length; i++ ){
            this.files.push(e.dataTransfer.files[i]);
          }
        }.bind(this));
      }
    },

    hasAbilityDragAndDrop(){
      let div = document.createElement('div');

      return (( 'draggable' in div) || ('ondragstart' in div && 'ondrop' in div))
              && 'FormData' in window
              && 'FileReader' in window;
    },

    openFileDialog(){
      this.$refs.fileInput.click();
    },

    removeFile(event) {
      this.files.splice(event, 1);
    },

    handleFileUpload() {
      this.files = this.files.concat(Array.from(this.$refs.fileInput.files));
    },
  },
  watch: {
    files(files) {
      this.$emit('input', files);
    }
  }
}
</script>

<style>
  .drop-area{
    border: 2px dotted gray;
  }
</style>