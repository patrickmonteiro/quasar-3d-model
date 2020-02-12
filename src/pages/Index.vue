<template>
  <q-page class="container">
    <div class="q-pa-sm">
      <p class="text-body1 text-bold">.STL</p>
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
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-stl>
    </div>
    <q-separator />
    <div class="q-pa-sm">
      <p class="text-body1 text-bold">.OBJ</p>
      <q-btn
        round
        color="primary"
        icon="attach_file"
        push
        @click="$refs.inputUploadObj.click()" />

      <q-btn round color="primary" push icon="colorize" class="cursor-pointer q-ml-sm">
        <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-color v-model="hex" />
        </q-popup-proxy>
      </q-btn>

      <input
        class="hidden"
        type="file"
        ref="inputUploadObj"
        @change="uploadImageObj" />
    </div>
    <div class="full-height">
      <model-obj
        v-if="obj"
        :width="500"
        :src="obj"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-obj>
    </div>
  </q-page>
</template>

<script>
import { ModelStl, ModelObj } from 'vue-3d-model'
export default {
  name: 'PageIndex',
  components: {
    ModelStl,
    ModelObj
  },
  data () {
    return {
      stl: undefined,
      obj: undefined,
      show: false,
      hex: '#000000'
    }
  },
  methods: {
    uploadImage (e) {
      this.$q.loading.show({
        message: 'Load Model 3D'
      })
      const file = e.target.files[0]
      this.stl = URL.createObjectURL(file)
      // console.log(file.name.substring(file.name.lastIndexOf('.') + 1))
    },
    uploadImageObj (e) {
      this.$q.loading.show({
        message: 'Load Model 3D'
      })
      const file = e.target.files[0]
      this.obj = URL.createObjectURL(file)
      // console.log(file.name.substring(file.name.lastIndexOf('.') + 1))
    },
    finishLoadModel () {
      // console.log('chamou')
      this.$q.loading.hide()
    },
    errorModel () {
      alert('Error in File.')
      this.$q.loading.hide()
      this.stl = undefined
    }
  }
}
</script>
