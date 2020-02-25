<template>
  <q-page class="container">
    <div class="row q-col-gutter-md q-pt-sm q-px-sm">
      <q-select
        class="col-8"
        outlined
        v-model="fileType"
        :options="options"
        dense
        label="Format"
        @input="resetType()" />
      <div class="col-4">
        <q-btn
          label="File"
          color="primary"
          icon="attach_file"
          class="full-width"
          push
          dense
          :disable="!fileType"
          @click="$refs.inputUpload.click()" />
      </div>
      <input
        class="hidden"
        type="file"
        :accept="`.${fileType}`"
        ref="inputUpload"
        @change="uploadImage" />
    </div>
    <div class="row" v-if="controls">
      <q-btn
        v-if="!rotateStatus"
        label="Rotate"
        color="secondary"
        icon="loop"
        class="col-md-2"
        push
        dense
        :disable="!fileType"
        @click="rotateInit()" />
      <q-btn
        v-else
        label="Rotate"
        color="negative"
        icon="stop"
        class="col-md-2"
        push
        dense
        :disable="!fileType"
        @click="rotateStop()" />
      <q-btn
        label="Background"
        color="primary"
        push
        dense
        :disable="!fileType"
        icon="colorize"
        class="cursor-pointer q-ml-sm col-md-2">
        <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-color v-model="hex" />
        </q-popup-proxy>
      </q-btn>
    </div>
      <model-stl
        v-if="fileType === 'stl' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-stl>
      <model-obj
        v-if="fileType === 'obj' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-obj>
      <model-collada
        v-if="fileType === 'dae' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-collada>
      <model-three
        v-if="fileType === 'json' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-three>
      <model-ply
        v-if="fileType === 'ply' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
        @on-load="finishLoadModel()"
        @on-error="errorModel()">
      </model-ply>
      <model-gltf
        v-if="fileType === 'gltf' && fileUrl"
        :width="500"
        :src="fileUrl"
        :backgroundColor="hex"
        :rotation="rotation"
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
      hex: '#ffffff',
      options: ['stl', 'obj', 'dae', 'json', 'ply', 'gltf'],
      rotation: {
        x: -Math.PI / 2,
        y: 0,
        z: 0
      },
      rotateStatus: false,
      controls: false
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
        this.controls = false
      }
    },
    finishLoadModel () {
      this.$q.loading.hide()
      this.controls = true
    },
    errorModel () {
      alert('Error in File.')
      this.$q.loading.hide()
      this.fileUrl = undefined
    },
    resetType () {
      this.fileUrl = undefined
    },
    rotateInit () {
      this.rotateStatus = true
      this.rotate()
    },
    rotateStop () {
      this.rotateStatus = false
    },
    rotate () {
      this.rotation.z += 0.01
      if (!this.rotateStatus) {
        cancelAnimationFrame(this.rotate)
        return
      }
      requestAnimationFrame(this.rotate)
    }
  }
}
</script>
