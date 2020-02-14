<template>
  <q-page class="container">
    <div class="row q-col-gutter-md q-pt-sm">
      <q-select class="col-8" outlined v-model="fileType" :options="options" dense label="Format" @input="resetType()" />
      <div class="col-4">
          <q-btn
            label="Select File"
            color="primary"
            icon="attach_file"
            push
            :disable="fileType"
            @click="$refs.inputUpload.click()" />
      </div>
      <!-- <div class="col-2">
        <q-btn round color="primary" push icon="colorize" class="cursor-pointer q-ml-sm">
          <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-color v-model="hex" />
        </q-popup-proxy>
      </q-btn>
      </div> -->
      <input
        class="hidden"
        type="file"
        :accept="`.${fileType}`"
        ref="inputUpload"
        @change="uploadImage" />
    </div>
      <model-stl
        v-if="fileType === 'stl' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-stl>
      <model-obj
        v-if="fileType === 'obj' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-obj>
      <model-collada
        v-if="fileType === 'dae' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-collada>
      <model-three
        v-if="fileType === 'json' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-three>
      <model-ply
        v-if="fileType === 'ply' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-ply>
      <model-gltf
        v-if="fileType === 'gltf' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundAlpha="0"
        :backgroundColor="hex"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-gltf>
  </q-page>
</template>

<script>
import { ModelStl, ModelObj, ModelCollada, ModelThree, ModelPly, ModelGltf } from 'vue-3d-model'
export default {
  name: 'PageIndex',
  components: {
    ModelStl,
    ModelObj,
    ModelCollada,
    ModelThree,
    ModelPly,
    ModelGltf
  },
  data () {
    return {
      fileUrl: undefined,
      fileType: '',
      show: false,
      hex: '#000000',
      options: ['stl', 'obj', 'dae', 'json', 'ply', 'gltf']
    }
  },
  methods: {
    uploadImage (e) {
      if (this.fileType) {
        this.$q.loading.show({
          message: 'Load Model 3D'
        })
        const file = e.target.files[0]
        this.fileUrl = URL.createObjectURL(file)
      }
    },
    finishLoadModel () {
      this.$q.loading.hide()
    },
    errorModel () {
      alert('Error in File.')
      this.$q.loading.hide()
      this.fileUrl = undefined
    },
    resetType () {
      this.fileUrl = undefined
    }
  }
}
</script>
