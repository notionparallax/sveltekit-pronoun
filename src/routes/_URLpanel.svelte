<script>
	export let url = '';
	export let altText = "alt text isn't working for some reason";
	export let person;

	import Badge from './_Badge.svelte';
</script>

<div class="the-url">
	<a class="display-url" href={url} target="_blank"
		><code>{@html url.replace(/(\/+|\?|\&)/gm, `$1<wbr>`)}</code></a
	>
	<p>
		Copy this text and paste it into <a
			href="https://github.com/notionparallax"
			title="this one is mine">your</a
		>
		<a
			href="https://docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme"
			title="here's how to make one of those">GitHub profile repo</a
		>, or wherever you can write <span class="smallcaps">html</span> or markdown.
	</p>
	<figure>
		<Badge cssClass={'live-badge'} {...person} />
		<!-- <img class="live-badge" alt="A pronoun badge that reads {altText}" src={url} /> -->
		<figcaption>This is the generated badge at that URL.</figcaption>
	</figure>
</div>

<!-- Alt text: {altText} -->
<style>
	.the-url {
		background: rgba(255, 255, 255, 0.8);
		margin: 1rem 2rem;
		border-radius: 2rem;
		padding: 1rem;
	}
	a.display-url {
		text-decoration: none;
		font-weight: bold;
	}
	.live-badge {
		max-width: 100%;
	}
	.smallcaps {
		font-variant-caps: all-small-caps;
	}
	.the-url {
		margin: 1rem 2rem;
		border-radius: 2rem;
		padding: 1rem;
	}
	.the-url code {
		font-size: 150%;
	}
	@media screen and (min-width: 650px) {
		.the-url code {
			font-size: 200%;
		}
		figure {
			margin: 0;
		}
		img {
			max-width: 100%;
		}
	}
</style>
