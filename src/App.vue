<script setup lang="ts">
</script>

<template>
  <main class=" min-h-screen dark:bg-gray-900">
    <section class="text-gray-600 body-font dark:text-gray-300">
      <div class="container px-5 py-24 mx-auto">
        <div class="xl:w-1/2 lg:w-3/4 w-full mx-auto text-center">
          <picture>
            <source srcset="./assets/dog.webp" type="image/webp">
            <source srcset="./assets/dog.png" type="image/jpeg">
            <img class=" rounded-b-full w-20 h-20 object-center inline-block mb-6 border-gray-200 hover:animate-spin"
              src="./assets/dog.png" alt="dog head">
          </picture>
          <p class="leading-relaxed text-lg">Hello <span class="wave">👋</span>, 42 is an answer for everything.</p>
          <span class="inline-block h-1 w-10 rounded bg-indigo-500 mt-8 mb-6"></span>
          <h2 class="text-gray-900 dark:text-gray-300  font-medium title-font tracking-wider text-sm">XIAOBO</h2>
          <p class="text-gray-500 dark:text-gray-400">Developer</p>
          <hr class=" m-8 ">
          <p class=" text-center tracking-normal text-slate-500 dark:text-slate-400">
            Find me on my
            <a class="hover:underline text-blue-500 dark:text-blue-400 after:content-['_↗']"
              href="https://blog.tomcat.run">Blog</a>
            ,
            <a class="hover:underline text-blue-500 dark:text-blue-400 after:content-['_↗']"
              href="https://github.com/bxb100">GitHub</a>
            ,
            <a class="hover:underline text-blue-500 dark:text-blue-400 after:content-['_↗']"
              href="https://twitter.com/meili145">Twitter</a>
            or
            <a class="hover:underline text-blue-500 dark:text-blue-400  after:content-['_↗']"
              href="https://space.bilibili.com/4432478">哔哩哔哩</a>
          </p>
          <p class="text-center tracking-normal text-slate-500 dark:text-slate-400">
            Mail me at <a class=" hover:underline" href="mailto:xiaobo@tomcat.run">
              xiaobo@tomcat.run
            </a>
          </p>
        </div>
      </div>
    </section>
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
</style>
