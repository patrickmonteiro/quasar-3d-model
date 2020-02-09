<template>
  <q-page class="container">
    <div>
      <q-uploader
      url="http://localhost:4444/upload"
      dark
      ref="img"
      @added="uploadImage"
    />
      <model-stl v-if="show" :src="stl" :backgroundColor="'black'"></model-stl>
    </div>
  </q-page>
</template>

<script>
import { ModelStl } from 'vue-3d-model'
export default {
  name: 'PageIndex',
  components: {
    ModelStl
  },
  data () {
    return {
      show: false,
      stl: undefined
    }
  },
  methods: {
    uploadImage () {
      const self = this
      var file = this.$refs.img.files[0]
      if (file !== undefined) {
        var reader = new FileReader()
        reader.onload = function (e) {
          self.stl = e.target.result
          self.show = true
          console.log(self.show)
        }
        reader.onerror = function (error) {
          alert(error)
        }
        reader.readAsDataURL(file)
      }
    }
  }
}
</script>
