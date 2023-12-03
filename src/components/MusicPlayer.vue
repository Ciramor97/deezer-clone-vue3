<template>
  <div
    id="MusicPlayer"
    v-if="audio"
    class="fixed flex min-w-[1000px] items-center justify-between bottom-0 w-full z-50 h-[80px] bg-[#23232D] border-t border-t-[#383838]"
  ></div>
</template>

<script setup>
import { ref, watch, onMounted } from "vue";

import MusicPlayerVolume from "../components/MusicPlayerVolume.vue";

import ShuffleVariant from "vue-material-design-icons/ShuffleVariant.vue";
import HeartOutline from "vue-material-design-icons/HeartOutline.vue";
import MicrophoneVariant from "vue-material-design-icons/MicrophoneVariant.vue";
import Plus from "vue-material-design-icons/Plus.vue";
import Tune from "vue-material-design-icons/Tune.vue";
import PictureInPictureBottomRight from "vue-material-design-icons/PictureInPictureBottomRight.vue";
import Play from "vue-material-design-icons/Play.vue";
import Pause from "vue-material-design-icons/Pause.vue";
import SkipBackward from "vue-material-design-icons/SkipBackward.vue";
import SkipForward from "vue-material-design-icons/SkipForward.vue";
import VolumeHigh from "vue-material-design-icons/VolumeHigh.vue";
import VolumeMute from "vue-material-design-icons/VolumeMute.vue";

import lyrics from "../lyrics.json";

import uniqolor from "uniqolor";

import { useSongStore } from "../stores/song";
import { storeToRefs } from "pinia";
const useSong = useSongStore();
const {
  isPlaying,
  audio,
  currentTrack,
  currentArtist,
  trackTime,
  isLyrics,
  currentVolume,
} = storeToRefs(useSong);

let randColor = ref("");
randColor.value = uniqolor.random();
let isHover = ref(false);
let isVolumeHover = ref(false);
let isTrackTimeCurrent = ref("0:00");
let isTrackTimeTotal = ref("0:00");
let seeker = ref(null);
let seekerContainer = ref(null);
let range = ref(0);

onMounted(() => {
  if (audio.value) {
    setTimeout(() => {
      timeupdate();
      loadmetadata();
    }, 300);
  }
  if (currentTrack.value) {
    seeker.value.addEventListener("change", function () {
      const time = audio.value.duration * (seeker.value.value / 100);
      audio.value.currentTime = time;
    });
    seeker.value.addEventListener("mousedown", function () {
      audio.value.pause();
      isPlaying.value = false;
    });
    seeker.value.addEventListener("mouseup", function () {
      audio.value.play();
      isPlaying.value = true;
    });
    seekerContainer.value.addEventListener("click", function (e) {
      const clickPosition =
        (e.pageX - seekerContainer.value.offsetLeft) /
        seekerContainer.value.offsetWidth;
      const time = audio.value.duration * clickPosition;
      audio.value.currentTime = time;
      seeker.value.value =
        (100 / audio.value.duration) * audio.value.currentTime;
    });
  }
});
</script>

<style>
.rangeDotHidden[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 0;
  height: 0;
}

.rangeDot[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  background-color: white;
  border-radius: 100%;
  width: 12px;
  height: 12px;
}
</style>
