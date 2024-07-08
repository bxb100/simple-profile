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
      .typeString("别问, 问就是 <strong>HashMap</strong>")
      .pauseFor(2000)
      .deleteAll()
      .typeString("完全不会写 ")
      .typeString("<strong>Java</strong>")
      .pauseFor(2000)
      .deleteChars(4)
      .typeString('<strong>Rust</strong>')
      .pauseFor(2000)
      .deleteChars(4)
      .typeString('<strong>Javascript</strong>')
      .pauseFor(2000)
      .start();
})

const app = ref()

function toggleStarField() {
  app.value.classList.toggle('show')
}

onMounted(() => {
  const controller = new AbortController()
  const timeoutId = setTimeout(() => controller.abort(), 3000)
  fetch("https://google.com", {mode: "no-cors", signal: controller.signal})
      .then(() => {
        console.log("fetch successful")
      })
      .catch(() => {
        document.getElementById("footer")?.classList.remove("hidden")
      })
      .finally(() => {
        clearTimeout(timeoutId)
      })
})

</script>

<template>
  <main class="dark:bg-gray-900">
    <!-- if there no border, the Mac's chrome will display different background color (small opacity diff)-->
    <div id="app" ref="app" class="w-screen h-screen border-2 dark:border-gray-900">
      <section class="text-gray-600 w-full h-full flex content-center items-center body-font">
        <div class="container px-5 py-24 mx-auto">
          <div class="xl:w-1/2 lg:w-3/4 w-full mx-auto text-center">
            <picture @mouseenter="toggleStarField" @mouseleave="toggleStarField">
              <source srcset="./assets/dog.webp" type="image/webp">
              <source srcset="./assets/dog.png" type="image/jpeg">
              <img class="rounded-b-full w-20 h-20 object-center inline-block mb-6 border-gray-200"
                   src="./assets/dog.png" alt="dog head">
            </picture>
            <p class="leading-relaxed text-lg dark:text-slate-400">
              <span ref="el"></span>
            </p>
            <span class="inline-block h-1 w-10 rounded bg-indigo-500 mt-8 mb-6"></span>
            <h2 class="text-gray-900 dark:text-gray-300  font-medium title-font tracking-wider">XiaoBo</h2>
            <p class="text-gray-500 dark:text-gray-400">后端开发</p>
<!--            <hr class=" m-8 ">-->
            <!--<p class=" tracking-normal text-slate-500 dark:text-slate-400">
              <a class="hover:underline" href="mailto:john@tomcat.run">
                john@tomcat.run
              </a>
            </p>-->
<!--            <p class=" text-center tracking-normal text-slate-500 dark:text-slate-400">-->
<!--              Find me on-->
<!--              <a class="hover:underline font-bold decoration-blue-500"-->
<!--                 href="https://blog.tomcat.run">Blog</a>-->
<!--              /-->
<!--              <a class="hover:underline font-bold decoration-sky-500"-->
<!--                 href="https://github.com/bxb100">GitHub</a>-->
<!--            </p>-->
          </div>
        </div>
      </section>
      <footer class="absolute bottom-6 left-0 right-0 w-36 mx-auto" aria-labelledby="footer-heading" id="footer">
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
  background-size: 100% 100% !important;
  background: url("assets/starfield.svg") no-repeat center;
}

</style>
