<script setup lang="ts">
import { Html5Qrcode } from "html5-qrcode"
import { onMounted, ref } from "vue";

const resString = ref('')
const init = () => {
  const html5QrCode = new Html5Qrcode('input-file')
  const input = document.getElementById('input-file') as HTMLInputElement
  input.addEventListener('change', e => {
    if (e.target && e.target.files.length == 0) {
      return
    }

    const imageFile = e.target.files[0]
    html5QrCode.scanFile(imageFile, true)
      .then(decodedText => {
        console.log('RESULT', decodedText)
        resString.value = decodedText
      })
      .catch(err => {
        console.log('Error scanning file', err)
      })
  })
}

onMounted(() => {
  init()
})
</script>

<template>
  <div>
    <input
      ref="input"
      type="file"
      id="input-file"
      accept="image/*"
      capture
    >
  </div>
  <div>
    <textarea>{{ resString }}</textarea>
  </div>
</template>