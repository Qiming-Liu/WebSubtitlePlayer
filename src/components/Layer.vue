<script>
import { $vfm, VueFinalModal, ModalsContainer } from "vue-final-modal";
import {
  UploadOutlined,
  PlayCircleTwoTone,
  PauseCircleTwoTone,
  PlusSquareFilled,
  MinusSquareFilled,
} from "@ant-design/icons-vue";
import Slider from "@vueform/slider";
import Srtparser from "srtparsejs";

import { defineComponent } from "vue";

export default defineComponent({
  components: {
    VueFinalModal,
    Slider,
    UploadOutlined,
    PlayCircleTwoTone,
    PauseCircleTwoTone,
    PlusSquareFilled,
    MinusSquareFilled,
  },
  props: {
    show: Boolean,
    setText: Function,
    setSize: Function,
  },
  data() {
    return {
      interval: 100,

      sliderValue: 0,
      max: 100,
      pause: false,
      intervaler: null,
      player: null,
      format: (sliderValue) => {
        let ms = sliderValue * this.interval;
        let time = Srtparser.toTime(ms);
        return time.substr(0, 8);
      },
    };
  },
  methods: {
    beforeUpload(file) {
      let t = this;
      let reader = new FileReader();

      reader.onload = function () {
        t.setSubtitle(this.result);
      };

      reader.readAsText(file);
      return false;
    },
    setSubtitle(srt) {
      //read srt file
      let srtArray = Srtparser.parse(srt);

      //set srtplayer
      this.player = Srtparser.setPlayer(srtArray, (text) => {
        this.setText(text);
      });

      //set max
      this.max = Math.floor(
        Srtparser.toMS(this.player.getEndTime()) / this.interval
      );

      if (this.intervaler === null) {
        // nothing to do
        this.intervaler = setInterval(() => {}, 60000);
        this.pause = true;
        this.player.update("00:00:00,000");
      } else {
        clearInterval(this.intervaler);
        this.pause = false;
        this.sliderValue = 0;

        //update time
        this.intervaler = setInterval(() => {
          if (this.pause) return;
          //update value
          this.sliderValue += 1;

          this.player.update(
            Srtparser.toTime(this.sliderValue * this.interval)
          );
        }, this.interval);
      }
    },
    togglePause(e) {
      this.pause = !this.pause;
      e.cancelBubble = true;
    },
  },
  mounted() {
    this.$nextTick().then(() => {
      this.setSubtitle(
        "1\n00:00:00,000 --> 00:00:01,000\nWeb Subtitle Player\n"
      );
    });
  },
});
</script>

<template>
  <keep-alive>
    <vue-final-modal
      v-model="show"
      classes="modal-container"
      content-class="modal-content"
    >
      <div class="upload">
        <a-upload
          accept=".srt"
          :beforeUpload="beforeUpload"
          :showUploadList="false"
        >
          <a-button>
            <upload-outlined></upload-outlined>Select .srt File
          </a-button>
        </a-upload>
      </div>

      <div class="size">
        <a-space>
          <a-button
            type="primary"
            v-on:click="
              (e) => {
                e.cancelBubble = true;
                setSize(1);
              }
            "
          >
            <template #icon>
              <plus-square-filled />
            </template>
          </a-button>
          <a-button
            type="primary"
            v-on:click="
              (e) => {
                e.cancelBubble = true;
                setSize(-1);
              }
            "
          >
            <template #icon>
              <minus-square-filled />
            </template>
          </a-button>
        </a-space>
      </div>

      <div class="play">
        <a-avatar
          :size="{ xs: 105, sm: 131, md: 164, lg: 205, xl: 256, xxl: 320 }"
          @click="togglePause"
          style="background: transparent"
        >
          <template #icon>
            <play-circle-two-tone v-if="pause" />
            <pause-circle-two-tone v-else />
          </template>
        </a-avatar>
      </div>

      <div class="slider">
        <Slider
          v-model="sliderValue"
          :format="format"
          :max="max"
          :lazy="false"
        />
      </div>
    </vue-final-modal>
  </keep-alive>
</template>

<style scoped>
.cover {
  height: 100vh;
  width: 100vw;
}
.upload {
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.size {
  position: absolute;
  top: 10%;
  left: 90%;
  transform: translate(-50%, -50%);
}
.play {
  position: absolute;
  top: 75%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.slider {
  position: absolute;
  width: 90%;
  top: 95%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.modal-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  position: relative;
  width: 50%;
  max-height: 300px;
  padding: 16px;
  overflow: auto;
  background-color: #fff;
  border-radius: 4px;
}
.modal-close {
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 32px;
  height: 32px;
  margin: 8px 8px 0 0;
  cursor: pointer;
}
.modal-close::hover {
  color: gray;
}
</style>
