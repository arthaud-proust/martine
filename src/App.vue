
<template>
  <div class="h-screen w-full flex justify-center">
    
    <div class="w-lg mx-auto py-4">
      <div class="mb-8">

        <button type="button" @click="open()" class="p-2 bg-neutral-300 rounded-md">
          Uploader l'image de couverture
        </button>
        
        <button type="button" @click="download()" class="ms-2 p-2 bg-neutral-300 rounded-md">
          Télécharger
        </button>
      </div>

      <article ref="cover" class="shadow-md border-l-cblue border-l-8 p-4 min-w-sm bg-white w-min">
        <header class="font-[VagRound] text-cblue w-min text-nowrap ">
          
          <h1 ref="name" contenteditable="true" class="w-min text-8xl ">
            martine
          </h1>
          <h2 ref="title" contenteditable="true" class="text-3xl text-right mb-1">Est à la montagne</h2>
        </header>
        
        <img :src="img" class="w-full"/>
      </article>
      </div>
  </div>
</template>

<script setup lang="ts">
import { useFileDialog } from '@vueuse/core';
import domtoimage from "dom-to-image";
import { ref, useTemplateRef } from 'vue';

const cover = useTemplateRef<HTMLDivElement>('cover')
const name = useTemplateRef<HTMLHeadingElement>('name')
const title = useTemplateRef<HTMLHeadingElement>('title')
const img = ref("/example.png")

const { open, onChange } = useFileDialog()
onChange((files) => {
  if(files) {
    const file = files[0]!

    img.value = URL.createObjectURL(file)
  }
})

const download = ()=>{
  const coverValue = cover.value
  const nameValue = name.value?.innerText ?? "martine"
  const titleValue = title.value?.innerText ?? "fait quelque chose"
  if(!coverValue) return

  console.log(coverValue)
  domtoimage.toJpeg(coverValue, { quality: 1 })
    .then(function (dataUrl) {
      console.log(dataUrl)
        const link = document.createElement('a');
        link.download = `${nameValue} ${titleValue}.jpeg`;
        link.href = dataUrl;
        link.click();
    })
    .catch(e=>console.log(e))
}
</script>
