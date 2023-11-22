<template>
  <div id="app">
    <video controls>
      <source type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <button @click="takePhoto">Take Photo</button>
    <div class="picture">
      <canvas></canvas>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  components: {},
  data: function () {
    return {
      stream: null
    }
  },
  methods: {
    init: async function () {
      if ('mediaDevices' in navigator && 'getUserMedia' in navigator.mediaDevices) {
        let constraints = {
          video: {
            width: {
              min: 640,
              ideal: 1280,
              max: 1920
            },
            height: {
              min: 360,
              ideal: 720,
              max: 1080
            }
          }
        };
        navigator.mediaDevices.getUserMedia(constraints).then(stream => {
          const videoPlayer = document.querySelector('video');
          videoPlayer.srcObject = stream;
          videoPlayer.play();
        })
      } else {
        alert("Cannot get Media Devices");
      }
    },
    takePhoto: function () {
      let ratio = (window.innerHeight < window.innerWidth) ? 16 / 9 : 9 / 16;
      const picture = document.querySelector('canvas');
      picture.width = (window.innerWidth < 1280) ? window.innerWidth : 1280;
      picture.height = window.innerWidth / ratio;
      const ctx = picture.getContext('2d');
      ctx.imageSmoothingEnabled = true;
      ctx.imageSmoothingQuality = 'high';

      // Draw the image without transformation
      ctx.drawImage(document.querySelector('video'), 0, 0, picture.width, picture.height);

      // Apply CSS transform conditionally based on device
      const isIOSDevice = /iPhone|iPad|iPod/i.test(navigator.userAgent);
      if (!isIOSDevice) {
        const pictureElement = document.querySelector('.picture');
        pictureElement.style.transform = 'scaleX(-1)';
      }
    }
  },
  beforeMount: function () {
    this.init();
  }
}
</script>

<style>
video {
  width: 100%;
  background: rgba(0, 0, 0, 0.2);
  transform: rotateY(180deg);
}
</style>
