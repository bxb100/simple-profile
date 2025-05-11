<template>
	<div>
		<div
			id="wrap"
			ref="el"
			@mousedown.prevent="start"
			@mouseup.prevent="back"
			:style="{ opacity: canvasOpacity, display: canvasDisplay }"
		>
		</div>

		<p class="info" v-if="!contentShown" :style="{ opacity: infoOpacity }">* Mouse or Finger press
			on the page to finish loading action.</p>

		<App v-if="showAppPage" :style="{ opacity: appPageOpacity }"/>
	</div>
</template>

<script setup>
import App from './App.vue'
import { nextTick, onMounted, onUnmounted, ref, useTemplateRef } from 'vue';
import * as THREE from 'three';
import { useFavicon, useTitle } from "@vueuse/core";

useTitle("Hi");
useFavicon("infinity.png")

// --- Reactive State ---
const toend = ref(false); // Controls the direction of the animation
const animatestep = ref(0); // Current step in the animation sequence
const acceleration = ref(0); // Calculated acceleration for animation effects
const contentShown = ref(false); // True when the transition to appPage begins

const infoOpacity = ref(1); // Opacity for the info text
const canvasContainerRef = useTemplateRef("el"); // Template ref for the Three.js canvas container
const canvasOpacity = ref(1); // Opacity for the canvas container
const canvasDisplay = ref('flex'); // Display style for canvas container, 'flex' to help center it

const showAppPage = ref(false); // Controls v-if for rendering appPage
const appPageOpacity = ref(0); // Opacity for the appPage

// --- Three.js Variables (will be initialized in onMounted) ---
let camera, scene, renderer, group, mesh, ringcover, ring;
let animationFrameId; // To store the ID from requestAnimationFrame

// --- Constants from original script ---
const CANVASS_SIZE = 500; // Renamed to avoid conflict, and made uppercase for constants
const TUBE_LENGTH = 30;   // Renamed for clarity
const TUBE_RADIUS = 5.6;  // Renamed for clarity
const ROTATE_VALUE = 0.035;
const PI2 = Math.PI * 2;

// --- Easing Function ---
// t: current time, b: beginning value, c: change in value, d: duration
function easing(t, b, c, d) {
	if ((t /= d / 2) < 1) return c / 2 * t * t + b;
	return c / 2 * ((t -= 2) * t * t + 2) + b;
}

// --- CustomTubeCurve Class for TubeGeometry ---
class CustomTubeCurve extends THREE.Curve {
	constructor(curveLength, curveRadius) {
		super();
		this.curveLength = curveLength;
		this.curveRadius = curveRadius;
	}

	getPoint(percent) {
		const currentLength = this.curveLength;
		const currentRadius = this.curveRadius;
		const x = currentLength * Math.sin(PI2 * percent);
		const y = currentRadius * Math.cos(PI2 * 3 * percent);
		let z, t_calc;

		t_calc = (percent % 0.25) / 0.25;
		t_calc = (percent % 0.25) - (2 * (1 - t_calc) * t_calc * -0.0185 + t_calc * t_calc * 0.25);

		if (Math.floor(percent / 0.25) === 0 || Math.floor(percent / 0.25) === 2) {
			t_calc *= -1;
		}
		z = currentRadius * Math.sin(PI2 * 2 * (percent - t_calc));

		if (isNaN(x) || isNaN(y) || isNaN(z)) {
			console.warn(`CustomTubeCurve.getPoint returned NaN for percent ${ percent }. Returning (0,0,0).`);
			return new THREE.Vector3(0, 0, 0);
		}
		return new THREE.Vector3(x, y, z);
	}
}

// --- Three.js Initialization Logic ---
function initThree() {
	// Camera setup
	camera = new THREE.PerspectiveCamera(65, 1, 1, 10000); // Aspect ratio 1 for square canvas
	camera.position.z = 150;

	// Scene setup
	scene = new THREE.Scene();
	group = new THREE.Group(); // Group to hold animated objects
	scene.add(group);

	// Create the tube path and mesh
	const tubePath = new CustomTubeCurve(TUBE_LENGTH, TUBE_RADIUS);
	mesh = new THREE.Mesh(
		new THREE.TubeGeometry(tubePath, 200, 1.1, 2, true),
		new THREE.MeshBasicMaterial({ color: 0xffffff })
	);
	group.add(mesh);

	// Create ring cover
	ringcover = new THREE.Mesh(
		new THREE.PlaneGeometry(50, 15, 1),
		new THREE.MeshBasicMaterial({ color: 0xd1684e, opacity: 0, transparent: true })
	);
	ringcover.position.x = TUBE_LENGTH + 1;
	ringcover.rotation.y = Math.PI / 2;
	group.add(ringcover);

	// Create ring
	ring = new THREE.Mesh(
		new THREE.RingGeometry(4.3, 5.55, 32),
		new THREE.MeshBasicMaterial({ color: 0xffffff, opacity: 0, transparent: true })
	);
	ring.position.x = TUBE_LENGTH + 1.1;
	ring.rotation.y = Math.PI / 2;
	group.add(ring);

	// Create fake shadows
	for (let i = 0; i < 10; i++) {
		const plain = new THREE.Mesh(
			new THREE.PlaneGeometry(TUBE_LENGTH * 2 + 1, TUBE_RADIUS * 3, 1),
			new THREE.MeshBasicMaterial({ color: 0xd1684e, transparent: true, opacity: 0.13 })
		);
		plain.position.z = -2.5 + i * 0.5;
		group.add(plain);
	}

	// Renderer setup
	renderer = new THREE.WebGLRenderer({ antialias: true });
	renderer.setPixelRatio(window.devicePixelRatio);
	renderer.setSize(CANVASS_SIZE, CANVASS_SIZE);
	renderer.setClearColor('#d1684e'); // Background color for the canvas

	// Append renderer's canvas to the designated container in the template
	canvasContainerRef.value?.appendChild(renderer.domElement);
}

// --- Animation and Render Logic ---
function renderThreeScene() {
	let progress;

	// Update animation step based on 'toend' state
	animatestep.value = Math.max(0, Math.min(240, toend.value ? animatestep.value + 1 : animatestep.value - 4));
	// Calculate acceleration using easing function
	acceleration.value = easing(animatestep.value, 0, 1, 240);

	// Apply animation effects based on acceleration
	if (acceleration.value > 0.35) {
		progress = (acceleration.value - 0.35) / 0.65;
		group.rotation.y = -Math.PI / 2 * progress;
		group.position.z = 50 * progress;

		progress = Math.max(0, (acceleration.value - 0.97) / 0.03);
		if (mesh) mesh.material.opacity = 1 - progress;
		if (ringcover) ringcover.material.opacity = progress;
		if (ring) ring.material.opacity = progress;
		if (ring) ring.scale.set(0.9 + 0.1 * progress, 0.9 + 0.1 * progress, 1);
	} else { // Reset or maintain initial states
		if (mesh) mesh.material.opacity = 1;
		if (ringcover) ringcover.material.opacity = 0;
		if (ring) ring.material.opacity = 0;
		if (animatestep.value === 0) { // Ensure reset to origin if animation is at the start
			group.rotation.y = 0;
			group.position.z = 0;
		}
	}

	// Render the scene
	if (renderer && scene && camera) {
		renderer.render(scene, camera);
	}

	// --- Content Transition Logic ---
	// Check if animation is at its end state and transition hasn't started
	if (toend.value && animatestep.value === 240 && !contentShown.value) {
		contentShown.value = true;

		infoOpacity.value = 0;

		canvasOpacity.value = 0;
		setTimeout(() => {
			canvasDisplay.value = 'none';

			showAppPage.value = true;

			nextTick(() => {
				appPageOpacity.value = 1;
			});

		}, 500); // This duration should match the CSS transition duration for #wrap
	}
}

// --- Main Animation Loop ---
function animate() {
	// If appPage is fully shown, stop the animation loop
	if (contentShown.value && appPageOpacity.value === 1) {
		if (animationFrameId) {
			cancelAnimationFrame(animationFrameId);
		}
		return;
	}

	// Rotate the main mesh
	if (mesh) {
		mesh.rotation.x += ROTATE_VALUE + acceleration.value;
	}

	renderThreeScene(); // Update scene state and render
	animationFrameId = requestAnimationFrame(animate); // Request next frame
}

// --- Event Handlers ---
// Handles click/touch to toggle the animation direction
function start() {
	toend.value = true;
}

function back() {
	toend.value = false;
}

// --- Vue Lifecycle Hooks ---
onMounted(() => {
	initThree(); // Initialize the Three.js scene, camera, objects, renderer
	animate();   // Start the animation loop

	// Add event listeners for user interaction
	document.body.addEventListener('mousedown', start, false);
	document.body.addEventListener('touchstart', start, false);
	document.body.addEventListener('mouseup', back, false);
	document.body.addEventListener('touchend', back, false);
});

onUnmounted(() => {
	// Clean up when the component is unmounted
	if (animationFrameId) {
		cancelAnimationFrame(animationFrameId);
	}

	// Remove event listeners
	document.body.removeEventListener('mousedown', start);
	document.body.removeEventListener('touchstart', start);
	document.body.removeEventListener('mouseup', back);
	document.body.removeEventListener('touchend', back);

	// Dispose of Three.js resources to prevent memory leaks
	if (renderer) {
		renderer.dispose();
		if (renderer.domElement.parentElement) {
			renderer.domElement.parentElement.removeChild(renderer.domElement);
		}
	}
	if (scene) {
		scene.traverse(object => {
			if (object.geometry) object.geometry.dispose();
			if (object.material) {
				if (Array.isArray(object.material)) {
					object.material.forEach(material => material.dispose());
				} else {
					object.material.dispose();
				}
			}
			// If you were to add textures, dispose them here:
			// if (object.texture) object.texture.dispose();
		});
	}
	// Any other specific cleanup for Three.js objects can be done here
});

</script>

<style>
body {
	background: #d1684e;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
	-webkit-touch-callout: none;
	-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

#wrap {
	position: absolute;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	overflow: hidden;
	transition: opacity 0.5s ease-in-out;
}

#wrap canvas {
	position: absolute;
	left: 50%;
	top: 50%;
	width: 500px;
	height: 500px;
	margin: -250px 0 0 -250px;
	-outline: 1px solid #fff;
}

.info {
	position: absolute;
	left: 0;
	right: 0;
	bottom: 0;
	font-size: 12px;
	color: #ccc;
	line-height: 2em;
	text-align: center;
}

</style>
