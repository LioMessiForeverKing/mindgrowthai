<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import Bounded from '$lib/components/Bounded.svelte';
	import { PrismicRichText } from '@prismicio/svelte';
	import type { Content } from '@prismicio/client';

	export let slice: Content.DemoHeroSlice;

	onMount(() => {
		const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

		if (prefersReducedMotion) {
			gsap.set('.demo__heading, .demo__body, .demo__video', {
				opacity: 1
			});
			return;
		}

		const tl = gsap.timeline({ defaults: { ease: 'power2.inOut' } });

		const observer = new IntersectionObserver((entries) => {
			entries.forEach(entry => {
				if (entry.isIntersecting) {
					tl.fromTo('.demo__heading', { scale: 0.5 }, { scale: 1, opacity: 1, duration: 1.4 });
					tl.fromTo('.demo__body', { y: 20 }, { y: 0, opacity: 1, duration: 1.2 }, '-=0.6');
					tl.fromTo('.demo__video', { y: 100 }, { y: 0, opacity: 1, duration: 1.3 }, '+=0.3');
					observer.disconnect();
				}
			});
		}, { threshold: 0.1 });

		const demoSection = document.querySelector('.demo');
		if (demoSection) {
			observer.observe(demoSection);
		}
	});
</script>

<style>
	.demo__heading {
		opacity: 0;
		transform-origin: center;
		font-size: 4rem;
		text-align: center;
		margin: 1rem 0;
	}

	.demo__body {
		opacity: 0;
		font-size: 2rem;
		margin: 1rem 0;
		text-align: center;
		color: "white";
	}

	.demo__video {
		opacity: 0;
		margin: 2rem 0;
	}
</style>

<Bounded class="demo" data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<div class="demo__heading" >
		<PrismicRichText field={slice.primary.heading} />
	</div>
	<div class="demo__body">
		<PrismicRichText field={slice.primary.body} />
	</div>
	<div class="demo__video">
		{@html slice.primary.demo_video.html}
	</div>
</Bounded>
