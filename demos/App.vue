<template lang="html">
  <div class="">
    <v-player
      :src="videoURL">
    </v-player>
  </div>
</template>

<script>
import Vplayer from '../src/vplayer.vue'

export default {
  data () {
    return {
      videoURL: 'http://static.smartisanos.cn/common/video/t1-ui.mp4'
    }
  },
  components: {
    'v-player': Vplayer
  }
}
</script>

<style lang="css">
</style>
