<template>
  <div id="app">
    <DropArea v-model="files"/>

  </div>
</template>

<script>
import DropArea from './components/DropArea.vue'

export default {
  name: 'app',
  components: {
    DropArea
  },
  data(){
    return {
      files: []
    }
  }
}
</script>
