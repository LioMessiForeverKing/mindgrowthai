<script lang="ts">
	import type { Content } from '@prismicio/client';
	import { PrismicRichText, PrismicImage } from '@prismicio/svelte';
	import clsx from 'clsx';
	import gsap from 'gsap';
	import { onMount } from 'svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import GoldText from './GoldText.svelte';
	import Heading2 from '$lib/components/Heading2.svelte';

	export let slice: Content.BentoSlice;

	// Function to observe elements
	function observeElements() {
		const elements = document.querySelectorAll('.animate-on-scroll');
		const options = {
			root: null,
			rootMargin: '0px',
			threshold: 0.6,
		};

		const observer = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) {
					// Use GSAP timeline for animation
					const tl = gsap.timeline({
						defaults: {
							duration: 2, // Set a slow animation duration
							ease: 'power2.out', // Use a nice ease
						},
					});

					tl.to(entry.target, {
						opacity: 1,
						y: 0,
					});
					observer.unobserve(entry.target); // Stop observing once animated
				}
			});
		}, options);

		elements.forEach((el) => {
			gsap.set(el, { opacity: 0, y: 20 }); // Initial state
			observer.observe(el);
		});
	}

	// Run the observer after the component is mounted
	onMount(() => {
		observeElements();
	});
</script>

<style>
	@keyframes flicker {
		0%, 100% {
			box-shadow: 0 0 15px 5px rgba(0, 255, 242, 0.6);
		}
		50% {
			box-shadow: 0 0 25px 10px rgba(255, 0, 170, 0.8);
		}
		30%, 70% {
			box-shadow: 0 0 20px 8px rgba(255, 0, 0, 0.7);
		}
		40%, 60% {
			box-shadow: 0 0 18px 6px rgba(166, 0, 255, 0.5);
		}
	}

	.glass-container {
		animation: flicker 2s infinite alternate; /* Flicker animation */
		background: rgba(255, 255, 255, 0.1); /* Keep the glass effect */
		backdrop-filter: blur(10px); /* Optional: Apply glass effect with blur */
		border-radius: 8px; /* Adjusted to match container's existing style */
	}
</style>

<Bounded data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<PrismicRichText
		field={slice.primary.heading}
		components={{ em: GoldText, heading2: Heading2 }}
	/>
	<div class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300 animate-on-scroll">
		<PrismicRichText field={slice.primary.body} />
	</div>

	<div class="mt-16 grid max-w-4xl grid-rows-[auto_auto_auto] gap-8 md:grid-cols-3 md:gap-10">
		{#each slice.primary.repeatable as item}
			<div
				class={clsx(
					'glass-container row-span-3 grid grid-rows-subgrid gap-4 rounded-lg bg-gray-950/60 p-4 before:bg-gray-100/10 animate-on-scroll',
					item.wide ? 'md:col-span-2' : 'md:col-span-1'
				)}
			>
				<h3 class="text-2xl">
					<PrismicRichText field={item.title} />
				</h3>
				<div class="max-w-md text-balance text-gray-300">
					<PrismicRichText field={item.body} />
				</div>
				<PrismicImage field={item.image} class="max-h-36 w-auto" />
			</div>
		{/each}
	</div>
</Bounded>
