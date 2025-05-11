<script setup lang="ts">
import { onMounted, ref } from "vue";
import Typewriter from "typewriter-effect/dist/core";
import { useFavicon, useTitle } from '@vueuse/core';

useTitle("Whoami");
useFavicon("favicon.svg")

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
	<main class="dark:bg-gray-900 bg-white select-none">
		<!-- if there no border, the Mac's chrome will display different background color (small opacity diff)-->
		<div id="app" ref="app" class="w-screen h-screen border-2 dark:border-gray-900">
			<section class="text-gray-600 w-full h-full flex content-center items-center body-font"
							 id="appSection">
				<div class="container px-5 py-24 mx-auto">
					<div class="xl:w-1/2 lg:w-3/4 w-full mx-auto text-center">
						<picture @mouseenter="toggleStarField" @mouseleave="toggleStarField">
							<source srcset="./assets/dog.webp" type="image/webp">
							<source srcset="./assets/dog.png" type="image/jpeg">
							<img class="rounded-b-full w-20 h-20 object-center inline-block mb-6 border-gray-200"
									 src="./assets/dog.png" alt="dog head">
						</picture>
						<p class="leading-relaxed text-lg dark:text-slate-400">
							Hello <span class="wave">👋</span>,
							<span ref="el"></span>
						</p>
						<span class="inline-block h-1 w-10 rounded bg-indigo-500 mt-8 mb-6"></span>
						<h2
							class="text-gray-900 dark:text-gray-300  font-medium title-font tracking-wider text-sm">
							Lemon</h2>
						<p class="text-gray-500 dark:text-gray-400">Developer</p>
						<hr class=" m-8 ">
						<!--<p class=" tracking-normal text-slate-500 dark:text-slate-400">
							<a class="hover:underline" href="mailto:john@tomcat.run">
								john@tomcat.run
							</a>
						</p>-->
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
/*noinspection CssUnusedSymbol*/
#app.show {
	background-size: 100% 60% !important;
	background: url("assets/starfield.svg") no-repeat center;;
}

</style>
