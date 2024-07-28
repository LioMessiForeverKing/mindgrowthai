<script>
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	const grid = [14, 30];
	const colors = ['#FF6F6F', '#FF9A9A', '#FFB6B6', '#FF6F9A']; // Color palette

	onMount(() => {
		const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

		if (prefersReducedMotion) {
			gsap.set('#triangle-grid', { opacity: 1 });
			gsap.set('.triangle-grid-item', {
				opacity: 0.2,
				scale: 1
			});
			return;
		}

		gsap.set('.triangle-grid-item', {
			opacity: 0,
			transformOrigin: 'center',
			color: colors[0] // Initial color
		});

		gsap.set('#triangle-grid', { opacity: 1 });

		const tl = gsap.timeline();

		// Wave Animation
		const waveAnimation = gsap.fromTo('.triangle-grid-item', 
			{
				y: (i, target) => Math.sin((i / grid[1]) * Math.PI * 2) * 10
			},
			{
				y: (i, target) => Math.sin((i / grid[1]) * Math.PI * 2) * -10,
				duration: 2,
				repeat: -1,
				yoyo: true,
				stagger: {
					amount: 0.1,
					grid: [grid[0], grid[1]],
					from: 'center'
				}
			}
		);

		// Entrance Animation
		tl.to('.triangle-grid-item', {
			keyframes: [
				{
					opacity: 0,
					duration: 0
				},
				{
					opacity: 0.6,
					rotate: '+=360',
					color: colors[1], // Transition to the second color
					scale: 2,
					duration: 1,
					stagger: {
						amount: 1.2,
						grid: grid,
						from: 'center'
					}
				},
				{
					opacity: 0.3,
					rotate: '+=360',
					color: colors[2], // Transition to the third color
					scale: 1,
					delay: -1,
					duration: 1,
					stagger: {
						amount: 1.2,
						grid: grid,
						from: 'center'
					}
				}
			]
		});

		// Loop Animation
		tl.to('.triangle-grid-item', {
			delay: 10,
			repeat: -1,
			repeatDelay: 10,
			keyframes: [
				{
					opacity: 0.6,
					rotate: '+=360',
					color: colors[3], // Transition to the fourth color
					scale: 2,
					duration: 1.2,
					stagger: {
						amount: 1.2,
						grid: grid,
						from: 'center'
					}
				},
				{
					opacity: 0.3,
					rotate: '+=360',
					color: colors[0], // Transition back to the first color
					scale: 1,
					delay: -1.2,
					duration: 1.2,
					stagger: {
						amount: 1.2,
						grid: grid,
						from: 'center'
					}
				}
			]
		});
	});
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	fill="none"
	viewBox="0 0 935 425"
	class="absolute -left-2 -top-14 -z-10"
	id="triangle-grid"
	opacity={0}
	style="mask-image: linear-gradient(black, transparent);"
>
	<g class="triangle-grid-group">
		{#each Array(grid[0]) as _, i}
			{#each Array(grid[1]) as __, j}
				<path
					fill="currentColor"
					opacity=".2"
					class="triangle-grid-item"
					d={`M${j * 32 + 5},${i * 32 + 10}l-3.75,2.165l0,-4.33l3.75,2.165z`}
				/>
			{/each}
		{/each}
	</g>
</svg>
