<script>
	import clsx from 'clsx';
	import { PrismicLink } from '@prismicio/svelte';

	/** @type {string | undefined} */
	let className = undefined;
	export { className as class };
</script>

<PrismicLink
	field={$$restProps.field}
	document={$$restProps.document}
	{...$$restProps}
	on:click
	class={clsx(
		'relative inline-flex h-fit w-fit rounded-full border border-violet-100/20 bg-violet-200/10 px-4 py-2 text-violet-200 outline-none ring-red-300 transition-colors after:absolute after:inset-0 after:-z-10 after:animate-pulse after:rounded-full after:bg-red-100 after:bg-opacity-0 after:blur-md after:transition-all after:duration-500 hover:border-red-200 hover:text-red-300 after:hover:bg-red-500 after:hover:bg-opacity-15 focus:ring-2',
		className
	)}
>
	<slot />
</PrismicLink>
