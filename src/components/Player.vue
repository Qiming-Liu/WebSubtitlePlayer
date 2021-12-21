<script>
import Background from './Background.vue';
import Text from './Text.vue';
import Slider from '@vueform/slider'
import srtparser from 'srtparsejs'
import { srt } from './srt'
export default {
  name: 'Player',
  components: {
    Background,
    Text,
    Slider
  },
  data() {
    return {
      size: "3",
      interval: 100,

      srtList: [],
      text: "",
      value: 0,
      max: 100,
      pause: false,
      format: value => {
        let ms = value * this.interval
        let time = srtparser.toTime(ms)
        return time.substr(0, 8)
      }
    }
  },
  mounted() {
    //read srt file
    let srtArray = srtparser.parse(srt);

    //creat srtplayer
    let player = srtparser.setPlayer(srtArray, text => {
      this.text = text;
    })

    //set max
    this.max = Math.floor(srtparser.toMS(player.getEndTime()) / this.interval)

    //update time
    setInterval(() => {
      if (this.pause) return;
      //update value
      this.value += 1

      player.update(srtparser.toTime(this.value * this.interval));
    }, this.interval);
  },
}
</script>

<template>
  <Background>
    <Text v-model:msg="text" :size="size" />
  </Background>
  <Slider v-model="value" :format="format" :max="max" :lazy="false" />
</template>

<style src="@vueform/slider/themes/default.css"></style>
<style scoped>
Slider {
  position: relative;
  width: 100vw;
  height: 20vh;
  margin-bottom: 10;
}
</style>