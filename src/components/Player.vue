<script>
import Background from './Background.vue'
import Text from './Text.vue';
import Layer from './Layer.vue';

import { defineComponent } from 'vue'

export default defineComponent({
  components: {
    Background,
    Text,
    Layer
  },
  data() {
    return {
      size: 3,

      text: "",
      showModal: true,
      setText: text => {
        this.text = text
      },
      setSize: change => {
        this.size += Number(change)
        if (this.size < 1) {
          this.size = 1
        } else if (this.size > 12) {
          this.size = 12
        }
      },
    }
  },
  methods: {
    toggleModal() {
      this.showModal = !this.showModal
    }
  }
})
</script>

<template>
  <Background @click="toggleModal">
    <Text v-model:msg="text" :size="size" />
  </Background>
  <Layer v-model:show="showModal" :setText="setText" :setSize="setSize" @click="toggleModal" />
</template>

<style src="@vueform/slider/themes/default.css"></style>
<style scoped>
</style>