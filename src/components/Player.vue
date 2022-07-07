<script>
import Background from "./Background.vue";
import Text from "./Text.vue";
import Layer from "./Layer.vue";

import { defineComponent } from "vue";

export default defineComponent({
  components: {
    Background,
    Text,
    Layer,
  },
  data() {
    return {
      size: 3,
      text: "",
      showModal: true,
      downTime: 0,
      setText: (text) => {
        this.text = text;
      },
      setSize: (change) => {
        this.size += Number(change);
        if (this.size < 1) {
          this.size = 1;
        } else if (this.size > 12) {
          this.size = 12;
        }
      },
    };
  },
  methods: {
    setTime() {
      this.downTime = Date.now();
      console.log(1);
    },
    toggleModal() {
      if (Date.now() - this.downTime < 200) {
        this.showModal = !this.showModal;
      }
    },
  },
});
</script>

<template>
  <Background @mousedown="setTime" @mouseup="toggleModal">
    <Text v-model:msg="text" :size="size" />
  </Background>
  <Layer
    v-model:show="showModal"
    :setText="setText"
    :setSize="setSize"
    @mousedown="setTime"
    @mouseup="toggleModal"
  />
</template>

<style src="@vueform/slider/themes/default.css"></style>
<style scoped></style>
