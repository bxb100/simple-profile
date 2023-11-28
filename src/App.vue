<script setup lang="ts">
import { onMounted, ref } from "vue";
import Typewriter from 'typewriter-effect/dist/core.js';

const el = ref()

onMounted(() => {
  const typewriter = new Typewriter(el.value, {
    loop: true,
    delay: 75
  });
  typewriter
      .typeString("<strong>42 is the answer for everything!</strong>")
      .pauseFor(2000)
      .deleteAll()
      .typeString("busy writing ")
      .typeString("<strong>Java</strong> bug")
      .pauseFor(2000)
      .deleteChars(8)
      .typeString('<strong>Rust</strong> bug')
      .pauseFor(2000)
      .deleteChars(8)
      .typeString('<strong>Javascript</strong> bug')
      .pauseFor(2000)
      .start();
})

const app = ref()

function toggleStarField() {
  app.value.classList.toggle('show')
}


</script>

<template>
  <main class="min-h-screen dark:bg-gray-900">
    <div id="app" ref="app" class="min-h-screen flex flex-col place-content-center">
      <section class="text-gray-600 body-font dark:text-gray-300 w-full">
        <div class="container px-5 py-24 mx-auto">
          <div class="xl:w-1/2 lg:w-3/4 w-full mx-auto text-center">
            <picture @mouseenter="toggleStarField" @mouseleave="toggleStarField">
              <source srcset="./assets/dog.webp" type="image/webp">
              <source srcset="./assets/dog.png" type="image/jpeg">
              <img class="rounded-b-full w-20 h-20 object-center inline-block mb-6 border-gray-200"
                   src="./assets/dog.png" alt="dog head">
            </picture>
            <p class="leading-relaxed text-lg">
              Hello <span class="wave">👋</span>,
              <span ref="el"></span>
            </p>
            <!--<p> <span>不闻不若闻之，闻之不若见之，见之不若知之，知之不若行之。学至于行之而止矣。</span></p>
            <p class="invisible"><span>《荀子·儒效》</span></p>-->
            <span class="inline-block h-1 w-10 rounded bg-indigo-500 mt-8 mb-6"></span>
            <h2 class="text-gray-900 dark:text-gray-300  font-medium title-font tracking-wider text-sm">XIAOBO</h2>
            <p class="text-gray-500 dark:text-gray-400">Developer</p>
            <hr class=" m-8 ">
            <p class="text-center tracking-normal text-slate-500 dark:text-slate-400">
              Mail me at <a class=" hover:underline" href="mailto:xiaobo@tomcat.run">
              xiaobo@tomcat.run
            </a>
            </p>
            <p class=" text-center tracking-normal text-slate-500 dark:text-slate-400">
              Find me on
              <a class="hover:underline font-bold decoration-blue-500"
                 href="https://blog.tomcat.run">Blog</a>
              /
              <a class="hover:underline font-bold decoration-sky-500"
                 href="https://github.com/bxb100">GitHub</a>
            </p>
          </div>
        </div>
      </section>
      <footer class="absolute bottom-1 left-0 right-0 w-36 mx-auto" aria-labelledby="footer-heading">
        <h2 id="footer-heading" class="sr-only">Footer</h2>
        <div class="flex justify-center">
          <a class="text-xs leading-5 text-gray-500 dark:text-gray-400 hover:text-indigo-500  "
             href="https://beian.miit.gov.cn/" target="_blank">皖ICP备2023022702号</a>
        </div>
      </footer>
    </div>

  </main>
</template>
<style>
.wave {
  animation-name: wave-animation;
  /* Refers to the name of your @keyframes element below */
  animation-duration: 2.5s;
  /* Change to speed up or slow down */
  animation-iteration-count: infinite;
  /* Never stop waving :) */
  transform-origin: 70% 70%;
  /* Pivot around the bottom-left palm */
  display: inline-block;
}

@keyframes wave-animation {
  0% {
    transform: rotate(0.0deg)
  }

  10% {
    transform: rotate(14.0deg)
  }

  /* The following five values can be played with to make the waving more or less extreme */
  20% {
    transform: rotate(-8.0deg)
  }

  30% {
    transform: rotate(14.0deg)
  }

  40% {
    transform: rotate(-4.0deg)
  }

  50% {
    transform: rotate(10.0deg)
  }

  60% {
    transform: rotate(0.0deg)
  }

  /* Reset for the last half to pause */
  100% {
    transform: rotate(0.0deg)
  }
}

/*
https://twitter.com/KaAnDK/status/1729149373043610098
https://codepen.io/amit_merchant/pen/MWPbaVG
 */
#app.show {
  height: 100%;
  background-size: 100% 100% !important;
  background: url("assets/starfield.svg") no-repeat center;
}


</style>
