<template>
  <q-page class="container">
    <div class="orientation-portrait text-center">
      <p class="text-h6">Por favor, vire o celular para visualizar</p>
      <p class="text-h6">Please turn the phone over to view.</p>
      <q-icon name="smartphone" size="10rem" class="rotate-90"/>
    </div>
    <div class="orientation-landscape">
      <model-stl v-if="show" src="statics/Modelo_Inferior.stl" :backgroundColor="'black'"></model-stl>
      <q-btn label="Show" icon="visibility" @click="show = true" class="full-width q-mt-lg" color="primary" />
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
      show: false
    }
  }
}
</script>
