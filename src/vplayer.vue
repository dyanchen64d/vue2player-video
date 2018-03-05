<template lang="html">
  <div class="player">
    <video ref="video" class="video">
      <source :src="src" type="video/mp4">
    </video>
    <controller
      :status="status"
      v-on:status-play="videoPlay"
      v-on:status-stop="videoStop"
      :duration="duration"
      :currtime="currtime"
      :statusSound="statusSound"
      v-on:sound-status-on="soundOn"
      v-on:sound-status-off="soundOff"
      v-on:change-progress="changeProgress"></controller>
  </div>
</template>

<script>
import Controller from './components/controller.vue'

export default {
  components: {
    Controller
  },
  data () {
    return {
      status: 'stop',
      duration: 0,
      currtime: 0,
      currTimeInterval: '',
      statusSound: 'on'
    }
  },
  props: {
    src: {
      type: String,
      required: true
    },
    poster: {
      type: String,
      default: 'posterimage.jpg'
    }
  },
  methods: {
    videoPlay () {
      this.status = 'play'
      this.$refs.video.play()
      this.currTimeInterval = window.setInterval(() => {
        // console.log((this.$refs.video.currentTime).toFixed(0));
        this.currtime = this.$refs.video.currentTime
      }, 200)
    },
    videoStop () {
      this.status = 'stop'
      this.$refs.video.pause()
      window.clearInterval(this.currTimeInterval)
    },
    soundOn () {
      this.statusSound = 'on'
      this.$refs.video.muted = false
    },
    soundOff () {
      this.statusSound = 'off'
      this.$refs.video.muted = true
    },
    changeProgress (percent) {
      this.$refs.video.currentTime = percent * this.duration
    }
  },
  mounted () {
    this.$refs.video.addEventListener('loadedmetadata', () => {
      this.duration =this.$refs.video.duration
    })
  },
  watch: {
    currtime (val, oVal) {
      if (val >= this.duration) {
        this.status = 'stop'
      }
    }
  }
}
</script>

<style lang="css">
.player {
  max-width: 640px;
  height: auto;
  position: relative;
}

.video {
  width: 100%;
  height: 100%;
}
</style>
