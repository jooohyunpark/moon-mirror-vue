<template>
  <div>
    <video width="640" height="480" autoplay></video>

    <div class="mirror">
      <div v-for="i in row" class="row" :id="i" :key="'row-' + i">
        <div v-for="i in column" class="column" :id="i" :key="'column-' + i">
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
      column: 16,
      row: 12,
      video: {}
    }
  },

  mounted() {
    this.initVideo()
    this.update()
  },
  methods: {
    initVideo() {
      const video = document.querySelector('video')
      this.video = {
        src: video,
        width: video.getBoundingClientRect().width,
        height: video.getBoundingClientRect().height
      }

      if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
        // Not adding `{ audio: true }` since we only want video
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then(function(stream) {
            video.srcObject = stream
            // video.play()
          })
      }
    },
    update() {
      const gridSize = document.querySelector('.column').getBoundingClientRect()
        .width

      for (let y = 0; y < this.video.height; y += gridSize) {
        for (let x = 0; x < this.video.width; x += gridSize) {
          // let i = y * gridSize * this.column + (gridSize * this.column - x - 1)
          let r = this.getPixelData(x, y)
          console.log(r)
        }
      }

      //  for (let y = 0; y < img.height; y += interval) {
      //           for (let x = 0; x < img.width; x += interval) {
      //               let i = y * width + (img.width - x - 1);
      //               let r = (255 - img.pixels[i * 4]) / 255;
      //               let g = (255 - img.pixels[i * 4 + 1]) / 255;
      //               let b = (255 - img.pixels[i * 4 + 2]) / 255;
      //               let darkness = (r + g + b) / 3;
      //               let radius = 20 * darkness;
      //               fill(0);
      //               ellipse(x + interval / 2, y + interval / 2, radius, radius);
      //           }
      //       }
    },
    getPixelData(x, y) {
      const canvas = document.createElement('canvas')
      canvas.width = this.video.width
      canvas.height = this.video.height
      canvas
        .getContext('2d')
        .drawImage(this.video.src, 0, 0, this.video.width, this.video.height)
      const pixelData = canvas.getContext('2d').getImageData(x, y, 1, 1).data

      return pixelData
    }
  }
}
</script>

<style lang="scss">
$gridSize: 60px;

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
