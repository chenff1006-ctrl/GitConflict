<template>
  <div id="app">
    <VideoPlayer />
  </div>
</template>

<script>
import VideoPlayer from './components/VideoPlayer.vue'

export default {
  name: 'app',
  components: {
    VideoPlayer
  }
}
</script>

<style>
html,body{
  width: 100%;
  height: 100%;
}
*{
  padding: 0;
  margin: 0;
}
#app {
  width: 100%;
  height: 100%;
}
</style>
