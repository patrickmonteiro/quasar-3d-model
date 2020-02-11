<template>
  <q-page class="container">
    <div class="q-pa-sm">
      <p class="text-body1 text-bold">STL</p>
      <q-btn
        round
        color="primary"
        icon="attach_file"
        push
        @click="$refs.inputUpload.click()" />

      <q-btn round color="primary" push icon="colorize" class="cursor-pointer q-ml-sm">
        <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-color v-model="hex" />
        </q-popup-proxy>
      </q-btn>

      <input
        class="hidden"
        type="file"
        ref="inputUpload"
        @change="uploadImage" />
    </div>
    <div class="full-height">
      <model-stl
        v-if="stl"
        :width="500"
        :src="stl"
        :backgroundColor="hex"></model-stl>
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
      stl: undefined,
      hex: '#000000'
    }
  },
  methods: {
    uploadImage (e) {
      const file = e.target.files[0]
      console.log(file.name.substring(file.name.lastIndexOf('.') + 1))
      this.stl = URL.createObjectURL(file)
    }
  }
}
</script>
