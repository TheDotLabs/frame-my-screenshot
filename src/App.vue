<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup

import { ref } from "@vue/reactivity";
import html2canvas from 'html2canvas';

const url = ref('');
const printcontent = ref(null);

const fileChange = (e: any) => {
  const file = e.target.files[0];
  url.value = URL.createObjectURL(file);
}

const printThis = async () => {
  console.log("printing..");
  const el = printcontent.value;

  const options = {
    type: "dataURL",
  };
  const printCanvas = await html2canvas(printcontent.value!, { backgroundColor: null });

  const link = document.createElement("a");
  link.setAttribute("download", "download.png");
  link.setAttribute(
    "href",
    printCanvas
      .toDataURL("image/png")
      .replace("image/png", "image/octet-stream")
  );
  link.click();

  console.log("done");
}
</script>

<template>
  <div class="relative">
    

    <div class="absolute flex w-full h-full">
      <!-- <div class="w-56 bg-gray-100">
        <div class="p-2 w-full hover:bg-gray-300 cursor-pointer">Screenshot</div>
        <div class="p-2 w-full hover:bg-gray-300 cursor-pointer">Style 2</div>
        <div class="p-2 w-full hover:bg-gray-300 cursor-pointer">Style 3</div>
      </div>-->
      <div class="w-full h-screen flex flex-col items-center justify-center">
        <div v-if="!url" class="mt-8 mb-2 text-5xl">Add macos like frames to your screenshot</div>
        <div v-if="!url" class="my-8 text-xl">Created with ❤️ by TheDotLabs</div>

        <div class="flex gap-4 mt-8">
          <input
            @change="fileChange"
            class="border rounded w-56"
            type="file"
            name="filename"
            accept="image/gif, image/jpeg, image/png"
          />
          <div
            v-if="url"
            class="bg-blue-500 py-1.5 px-3 text-sm font-bold text-white rounded shadow text-center hover:bg-blue-600 cursor-pointer"
            @click="printThis"
          >Download as PNG</div>
        </div>
        <div v-if="!url" class="mt-4 text-neutral-500">Click above to upload your image</div>

        <div ref="printcontent" v-if="url">
          <div class="rounded-lg overflow-hidden border border-neutral-300 shadow-2xl m-8">
            <div class="h-6 bg-neutral-100 relative">
              <div class="h-full flex items-center pl-2">
                <div class="h-2.5 w-2.5 rounded-full bg-red-500"></div>
                <div class="ml-2 h-2.5 w-2.5 rounded-full bg-yellow-500"></div>
                <div class="ml-2 h-2.5 w-2.5 rounded-full bg-green-500"></div>
              </div>
              <div
                class="text-xs text-center font-bold absolute -top-1 left-1/2 -translate-x-1/2 m-0 p-0"
              >Title</div>
            </div>
            <img class="object-contain" style="max-height: 70vh;" :src="url" />
          </div>
        </div>
      </div>
    </div>
    <div class="w-full h-12 absolute left-0 right-0 shadow">
      <div class="h-full flex items-center px-4 justify-between">
        <div
          class="font-semibold text-2xl"
          style="font-family: 'Caveat', cursive;"
        >Frame my Screenshot</div>
        <a class="rounded py-2 px-3 bg-neutral-100 hover:bg-neutral-300 cursor-pointer text-sm " href="https://github.com/thedotlabs" target="_blank">Github</a>
      </div>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
