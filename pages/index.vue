<template>
  <div>
    <video ref="video" autoplay playsinline></video>
    <button @click="capturePhoto">Capture Photo</button>
    <canvas ref="canvas" style="display: none;"></canvas>
    <img :src="photo" v-if="photo" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const video = ref(null)
const canvas = ref(null)
const photo = ref(null)

onMounted(async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true })
    video.value.srcObject = stream
  } catch (err) {
    console.error('Error accessing the camera:', err)
  }
})

const capturePhoto = () => {
  const context = canvas.value.getContext('2d')
  canvas.value.width = video.value.videoWidth
  canvas.value.height = video.value.videoHeight
  context.drawImage(video.value, 0, 0, canvas.value.width, canvas.value.height)
  photo.value = canvas.value.toDataURL('image/png')
}
</script>

<style scoped>
video {
  width: 100%;
  max-width: 600px;
}
img {
  margin-top: 20px;
  max-width: 100%;
}
</style>
