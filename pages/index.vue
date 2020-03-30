<template>
  <div>
    <div class="container"></div>
    <video width="640" height="480" autoplay></video>
  </div>
</template>

<script>
export default {
  mounted() {
    const video = document.querySelector('video')

    // Get access to the camera!
    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      // Not adding `{ audio: true }` since we only want video now
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then(function(stream) {
          video.srcObject = stream
          // video.play()
        })
    }
  },
  methods: {}
}
</script>

<style lang="scss">
*,
html {
  margin: 0;
  padding: 0;
}
body {
  background: #000;
}

video {
  transform: rotateY(180deg);
  -webkit-transform: rotateY(180deg); /* Safari and Chrome */
  -moz-transform: rotateY(180deg); /* Firefox */
}
</style>
