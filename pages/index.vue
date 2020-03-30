<template>
  <div>
    <video width="640" height="480" autoplay></video>

    <div class="mirror">
      <div v-for="i in row" class="row" :id="i" :key="'row-' + i">
        <div v-for="i in column" class="column" :id="i" :key="'column-'+i">
          <img src="@/assets/img/moon.png" width="100%" alt />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      column: 10,
      row: 10
    }
  },
  mounted() {
    // this.initVideo()
  },
  methods: {
    initVideo() {
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
    }
  }
}
</script>

<style lang="scss">
$gridSize: 80px;

*,
html {
  margin: 0;
  padding: 0;
}

body {
  background: #000;
  color: #fff;

  video {
    transform: rotateY(180deg);
    -webkit-transform: rotateY(180deg); /* Safari and Chrome */
    -moz-transform: rotateY(180deg); /* Firefox */
  }

  .mirror {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    .row {
      display: flex;
    }

    .column {
      width: $gridSize;
      height: $gridSize;
    }
  }
}
</style>
