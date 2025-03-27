<template>
    <header class="flex flex-row w-full relative bg-gray-600 border-b-2 border-gray-400">
        <div class="basis-1/4 items-center justify-start px-8">
          <img src="assets/images/arizonalogo.png" class="w-16 h-16 bg-transparent" />
        </div>
        <div class="basis-1/4"></div>
        <nav :class="{'basis-1/2 flex flex-row items-center justify-end px-8 gap-4': !burger || burger, 'max-sm:basis-auto max-sm:absolute max-sm:top-full max-sm:w-full max-sm:justify-center max-sm:flex-col max-sm:gap-0 max-sm:bg-white max-sm:px-0': burger, 'max-sm:hidden': !burger}">
            <NuxtLink to="/" class="my-auto p-2 hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:border-b-2 max-sm:border-black max-sm:text-center">Home</NuxtLink>
            <div class="my-auto p-2 hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:px-0 max-sm:pb-0" @click='switch_submenu'>Labs
              <div class="flex flex-col absolute top-full bg-white text-black w-56 text-center max-sm:relative max-sm:w-full max-sm:bg-red-100">
                <NuxtLink to="/labs" class="my-auto p-2 border-b-2 border-black hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:border-t-2 max-sm:text-center">Lab1</NuxtLink>
                <NuxtLink to="/labs" class="my-auto p-2 border-b-2 border-black hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:text-center">Lab2</NuxtLink>
                <NuxtLink to="/labs" class="my-auto p-2 border-b-2 border-black hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:text-center">Lab3</NuxtLink>
              </div>
            </div>
            <NuxtLink to="/login" class="my-auto p-2 hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:border-b-2 max-sm:border-black max-sm:text-center">LogIn</NuxtLink>
            <NuxtLink to="/logout" class="my-auto p-2 hover:bg-gray-500 hover:text-white max-sm:w-full max-sm:border-b-2 max-sm:border-black max-sm:text-center">LogOut</NuxtLink>
        </nav>
        <div v-if="!burger" @click="switch_burger" class="max-sm:flex max-sm:flex-col max-sm:mr-8 max-sm:justify-between max-sm:items-center max-sm:w-8 max-sm:h-6">
            <span class="h-[3px] w-full bg-black-950"></span>
            <span class="h-[3px] w-full bg-black-950"></span>
            <span class="h-[3px] w-full bg-black-950"></span>
        </div>
        <div v-else @click="switch_burger" class="max-sm:flex max-sm:flex-col max-sm:mr-8 max-sm:justify-between max-sm:items-center max-sm:w-8 max-sm:h-6">
          <span class="h-[3px] w-full bg-black-950 rotate-45 relative top-[9px]"></span>
          <span class="h-[3px] w-full bg-black-950 opacity-0"></span>
          <span class="h-[3px] w-full bg-black-950 realtive bottom-3 rotate-[-45deg]"></span>
        </div>
    </header>
    
    <main class="p-5 flex bg-gray-400 h-screen">
        <slot />
    </main>
    
    <footer class="flex flex-row w-full border-t-2 justify-center px-4 py-2 gap-4">
      <a href="https://www.youtube.com/"><img src="assets/images/youtube.png" class="w-16 h-16" /></a>
      <a href="https://github.com/"><img src="assets/images/github.png" class="w-16 h-16" /></a>
      <a href="https://www.facebook.com/"><img src="assets/images/facebook.png" class="w-16 h-16 bg-transparent" /></a>
    </footer>
</template>

<script setup lang="ts">
import { useHead } from '#app';
import {ref} from 'vue';

useHead({
  script: [
    {
      async: true,
      src: 'https://www.googletagmanager.com/gtag/js?id=G-3N35SPZ3Z5',
    },
    {
      innerHTML: `
        window.dataLayer = window.dataLayer || [];
        function gtag(){dataLayer.push(arguments);}
        gtag('js', new Date());
        gtag('config', 'G-3N35SPZ3Z5');
      `,
      type: 'text/javascript',
    },
  ],
  __dangerouslyDisablesAntizersByTagID: {
    gtag: ['innerHTML'],
  },
});

const burger=ref<boolean>(false)
const submenu=ref<boolean>(false)

const switch_burger = (): void => {burger.value = !burger.value}
const switch_submenu =(): void => {submenu.value = !submenu.value}
</script>
444