<template>
    <div>
      <button @click="openCamera">Open Camera</button>
      <!-- <video ref="videoElement" v-if="isCameraOpen" autoplay></video> -->
      <video ref="videoElement" v-if="isCameraOpen" autoplay style="width: 100%; height: auto;"></video>
      <canvas ref="canvas" style="display: none;"></canvas>
      <button v-if="isCameraOpen" @click="takePhoto">Take Photo</button>
      <div v-if="capturedImage">
        <img :src="capturedImage" alt="Captured Image">
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  const isCameraOpen = ref(false);
  const capturedImage = ref(null);
  const videoStream = ref(null);
  const videoElement = ref(null);
  
  const openCamera = async () => {
    try {
      // const stream = await navigator.mediaDevices.getUserMedia({video: true});
      const stream = await navigator.mediaDevices.getUserMedia({
        video: {
          width: { ideal: 1280 },
          height: { ideal: 720 },
        }
      });
  
  
      if (videoElement.value) {
        videoElement.value.srcObject = stream;
        videoElement.value.play();
      }
  
      // videoElement.value = stream;
      isCameraOpen.value = true;
    } catch (error) {
      console.error('Error accessing the camera:', error);
    }
  }
  </script>
  
  <style>
  /* Add your styles here */
  </style>
  