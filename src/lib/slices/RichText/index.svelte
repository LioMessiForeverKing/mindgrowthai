<script lang="ts">
	import type { Content } from '@prismicio/client';
	import { PrismicRichText } from '@prismicio/svelte';
	import Label from './Label.svelte';
	import Bounded from '$lib/components/Bounded.svelte';

	export let slice: Content.RichTextSlice;
</script>

<Bounded class="container">
	<PrismicRichText
		field={slice.primary.content}
		components={{
			label: Label
		}}
	/>
	</Bounded>