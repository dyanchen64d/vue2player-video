<template lang="html">
  <div class="controller">
    <div class="progress-bar">
      <div class="progress-bar-color" ref="progressBar"></div>
      <div class="progress-bar-click" ref="progressBarTotal" @click="changeProgress"></div>
    </div>
    <div class="operationsWrapper">
      <div class="operLeft">
        <div class="play-stop">
          <img :src="src" alt="" @click="changePlayStatus">
        </div>
        <div class="curr-time">{{currtime.toFixed(0)}}</div>
        <div class="duraton">&nbsp;/&nbsp;{{duration.toFixed(0)}}</div>
      </div>
      <div class="operRight">
        <div class="sound" @click="changeSoundStatus">
          <img :src="soundSrc" alt="">
        </div>
        <div class="full-screen">
          <img :src="fullScreen" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      src: '/src/assets/play.png',
      playSrc: '/src/assets/play.png',
      stopSrc: '/src/assets/stop.png',
      soundSrc: '/src/assets/soundon.png',
      soundSrcOn: '/src/assets/soundon.png',
      soundSrcOff: '/src/assets/soundoff.png',
      fullScreen: '/src/assets/fullscreen.png'
    }
  },
  props: {
    status: {
      type: String,
      required: true
    },
    duration: {
      type: Number,
      default: 0
    },
    currtime: {
      type: Number,
      default: 0
    },
    statusSound: {
      type: String,
      require: true
    }
  },
  methods: {
    changePlayStatus () {
      if (this.status === 'stop') {
        // console.log('stop');
        this.$emit('status-play')
      } else if (this.status === 'play') {
        // console.log('play');
        this.$emit('status-stop')
      }
    },
    changeSoundStatus () {
      if (this.statusSound === 'on') {
        this.$emit('sound-status-off')
      } else if (this.statusSound === 'off') {
        this.$emit('sound-status-on')
      }
    },
    changeProgress (e) {
      // console.log(e);
      // console.log(this.$refs.progressBarTotal.clientWidth);
      let percent = e.offsetX / this.$refs.progressBarTotal.clientWidth
      this.$emit('change-progress', percent)
    }
  },
  watch: {
    status (val, oVal) {
      // console.log(val, oVal);
      if (val === 'play') {
        this.src = this.stopSrc
      } else if (val === 'stop') {
        this.src = this.playSrc
      }
    },
    statusSound (val, oVal) {
      if (val === 'on') {
        this.soundSrc = this.soundSrcOn
      } else if (val === 'off') {
        this.soundSrc = this.soundSrcOff
      }
    },
    currtime (val, oVal) {
      // console.log('' + (this.currtime / this.duration));
      this.$refs.progressBar.style.width = (this.currtime / this.duration * 100) + '%'
    }
  },
  mounted () {
    //
  }
}
</script>

<style lang="scss">
.controller {
  width: 100%;
  height: 40px;
  position: absolute;
  right: 0;
  bottom: 0px;
  display: flex;
  flex-direction: column;

  .progress-bar {
    width: 100%;
    height: 4px;
    background-color: grey;
    position: relative;

    .progress-bar-click {
      width: 100%;
      height: 4px;
      background-color: none;
      position: absolute;
      top: 0;
      left: 0;
      z-index: 2;
    }

    .progress-bar-color {
      width: 0;
      height: 4px;
      background-color: blue;
      position: absolute;
      top: 0;
      left: 0;
      z-index: 1;
    }
  }

  .operationsWrapper {
    flex: 1;
    background-color: none;
    display: flex;
    padding: 0 8px 0 8px;
    justify-content: space-between;
    align-items: center;
  }

  .operLeft {
    display: flex;
    align-items: center;
    font-size: 12px;

    .play-stop {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 4px;

      img {
        width: 24px;
        height: 24px;
      }
    }
  }

  .operRight {
    display: flex;
    justify-content: center;
    align-items: center;

    .sound {
      margin-right: 8px;
      position: relative;

      img {
        width: 28px;
        height: 28px;
      }
    }

    .full-screen {
      img {
        width: 18px;
        height: 18px;
      }
    }
  }
}


</style>
