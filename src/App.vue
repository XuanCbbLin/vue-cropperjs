<template>
  <div class="flex">
    <div>
      <vue-cropper ref="cropper" :src="cropperImgSrc" alt="berserk" preview=".preview" :aspect-ratio="1 / 1">
      </vue-cropper>
      <div>
        <button class="bg-blue-800 text-white py-2 px-3" @click="getCropper">crop</button>
        <button class="bg-blue-800 text-white ml-3 py-2 px-3" @click="resetCropper">reset</button>
        <button class="bg-blue-800 text-white ml-3 py-2 px-3" @click="zoomIn">Zoom in</button>
        <button class="bg-blue-800 text-white ml-3 py-2 px-3" @click="zoomOut">Zoom out</button>
        <input class="ml-3" type="file" name="image" accept="image/*" @change="setImage" />
      </div>
    </div>
    <div class="ml-2 w-[307px]">
      <p>Preview</p>
      <div class="rounded-full h-[300px] mb-3 w-[300px] overflow-hidden preview"></div>
      <p>Cropped Image</p>
      <div>
        <img v-if="cropedImg" :src="cropedImg" alt="Cropped Image" />
        <div v-else class="bg-gray-600 h-56 w-full" />
      </div>
    </div>
  </div>
</template>

<script setup>
import VueCropper from 'vue-cropperjs';
import 'cropperjs/dist/cropper.css';
import imgSrc from '../public/berserk.jpg';
import { ref } from 'vue';

const cropper = ref(null);
const cropperImgSrc = ref(imgSrc);
const cropedImg = ref('');

const getCropper = () => {
  cropedImg.value = cropper.value.getCroppedCanvas().toDataURL();
};

const resetCropper = () => {
  cropper.value.reset();
};

const setImage = e => {
  const file = e.target.files[0];
  const reader = new FileReader();

  reader.readAsDataURL(file);

  reader.addEventListener('load', event => {
    cropper.value.replace(event.target.result);
  });
};

const zoomIn = () => {
  cropper.value.cropper.zoom(0.1);
};

const zoomOut = () => {
  cropper.value.cropper.zoom(-0.1);
};
</script>

<style scoped></style>
