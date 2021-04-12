<script>
	import UrlDisplay from './UrlDisplay.svelte';

	let person = {
		subject: 'He',
		object: 'Him',
		posessive: 'His',
		posessivePronoun: 'His',
		reflexive: 'Himself',
		emoji: '👫',
		colour: '#B00B55',
		height: '60',
		sep: '|',
		note: ''
	};

	$: altText = makeText(person);
	$: url = buildURL(person);

	function _buildURL(person) {
		let parts = [];
		if (person.subject) parts.push(`subject=${person.subject}&`);
		if (person.object) parts.push(`object=${person.object}&`);
		if (person.posessive) parts.push(`posessive=${person.posessive}&`);
		if (person.posessivePronoun) parts.push(`posessive-pronoun=${person.posessivePronoun}&`);
		if (person.reflexive) parts.push(`reflexive=${person.reflexive}&`);
		if (person.emoji) parts.push(`emoji=${person.emoji}&`);
		if (person.colour) parts.push(`colour=${person.colour.replace('#', '')}&`);
		if (person.height) parts.push(`height=${person.height}&`);
		if (person.sep) parts.push(`sep=${person.sep}&`);
		if (person.note) parts.push(`sep=${person.note}&`);
		url = 'https://pronoun.cyou/x/y?' + parts.join('');
		if (url.slice(-1) == '&') {
			url = url.slice(0, -1);
		}
		return url;
	}
	function buildURL() {
		return _buildURL(person);
	}

	function _makeText(person) {
		let parts = [];
		if (person.subject) parts.push(person.subject);
		if (person.object) parts.push(person.object);
		if (person.posessive) parts.push(person.posessive);
		if (person.posessivePronoun) parts.push(person.posessivePronoun);
		if (person.reflexive) parts.push(person.reflexive);
		if (person.emoji) parts.push(person.emoji); // this seems to break sapper
		let text = parts.join(person.sep || '|');

		return text;
	}

	function makeText() {
		return _makeText(person);
	}

	import { onMount } from 'svelte';

	let photos = [];

	onMount(async () => {
		const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=4`);
		const theJson = await res.json();
		let examplePeople = [
			{
				subject: 'Zie',
				object: 'Zim',
				posessive: 'Zis',
				posessivePronoun: 'Zir',
				reflexive: 'Zieself',
				colour: '#00bcd4'
			},
			{
				subject: 'She',
				object: 'Her',
				posessive: 'Hers',
				posessivePronoun: 'Her',
				reflexive: 'Herself',
				colour: '#ffa000'
			},
			{
				subject: 'He',
				object: 'Him',
				posessive: 'His',
				posessivePronoun: 'His',
				reflexive: 'Himself',
				colour: '#9e9e9e'
			},
			{
				subject: 'Hep',
				object: 'Himp',
				posessive: 'His',
				posessivePronoun: 'His',
				reflexive: 'Himself',
				colour: '#009688'
			}
		];
		for (const p of examplePeople) {
			p.url = _buildURL(p);
			p.altText = _makeText(p);
			console.log(p);
		}

		theJson, examplePeople;
		let temp = [];
		theJson.forEach((e, i) => temp.push({ ...e, ...examplePeople[i] }));
		photos = temp;
	});
</script>

<svelte:head>
	<title>Pronoun Badge</title>
</svelte:head>
<div class="wrapper">
	<UrlDisplay {url} {altText} />

	<form>
		<p>
			Fill in this form with your pronouns. The code snippet above will update as you go.<br />
			If you don't want to include a section, just leave it blank.
		</p>
		<div class="input-group">
			<label for="subject">subject</label>
			<input name="subject" bind:value={person.subject} />
			<p class="note">
				{person.subject}
				{person.subject.toLocaleLowerCase() == 'they' ? 'are' : 'is'} amazing.
			</p>
		</div>

		<div class="input-group">
			<label for="object">object</label>
			<input name="object" bind:value={person.object} />
			<p class="note">Give an award to {person.object.toLocaleLowerCase()}.</p>
		</div>

		<div class="input-group">
			<label for="posessive">posessive</label>
			<input name="posessive" bind:value={person.posessive} />
			<p class="note">{person.posessive} brain is intimidating!</p>
		</div>

		<div class="input-group">
			<label for="posessive-pronoun">posessive pronoun</label>
			<input name="posessive-pronoun" bind:value={person.posessivePronoun} />
			<p class="note">
				{person.subject}
				thinks the cat is
				{person.posessivePronoun.toLocaleLowerCase()}
				but the cat thinks otherwise.
			</p>
		</div>

		<div class="input-group">
			<label for="reflexive">reflexive</label>
			<input name="reflexive" bind:value={person.reflexive} />
			<p class="note">{person.subject} thinks highly of {person.reflexive.toLocaleLowerCase()}.</p>
		</div>

		<div class="input-group">
			<label for="emoji">emoji</label>
			<input name="emoji" bind:value={person.emoji} />
			<p class="note">This/these emoji say something about you. {person.emoji}</p>
		</div>

		<div class="input-group">
			<label for="colour">colour</label>
			<input name="colour" bind:value={person.colour} type="color" />
			<p class="note">
				This is the background colour of the badge ({person.colour}: A hex colour)
			</p>
		</div>

		<div class="input-group">
			<label for="height">height</label>
			<input name="height" bind:value={person.height} />
			<p class="note">The image sent back will be {person.height}px high.</p>
		</div>

		<div class="input-group">
			<label for="sep">separator</label>
			<input name="sep" bind:value={person.sep} />
			<p class="note">
				Each section will be separated by this/these characters. e.g. 👁{person.sep}👄{person.sep}👁
			</p>
		</div>

		<!-- TODO: waiting fo`r the server to accept it -->
		<!-- <div class="input-group">
    <label for="note">note</label>
    <input name="note" bind:value={person.note} />
    <p class="note">
      note={person.note}
      a little note that will show up in the server logs. Who
      <em>are</em>
      you?
    </p>
  </div> -->
	</form>

	<div class="photos">
		{#each photos as photo}
			<figure class="example-person">
				<img
					class="mini-example"
					alt="A pronoun badge that reads {photo.altText}"
					src={photo.url}
				/><br />
				<img src={photo.thumbnailUrl} alt={photo.title} />
				<figcaption>
					{photo.subject},
					{photo.object},
					{photo.posessive},
					{photo.posessivePronoun},
					{photo.reflexive}
				</figcaption>
			</figure>
		{:else}
			<p data-comment="this block renders when photos.length === 0">loading...</p>
		{/each}
	</div>

	<div class="explainer">
		<h1>So, what is this?</h1>
		<p>
			This is a way for me, <a href="https://notionparallax.co.uk/me">Ben</a>, to learn the web
			framework
			<a href="https://svelte.dev/">Svelte</a>. It's also an attempt to make something useful in a
			series of one day bursts when I get sick of all my other projects. You can put this image
			<span class="smallcaps">URL</span> anywhere that you can control the markup. The obvious place
			is in your GitHub profile repo.
		</p>
	</div>
</div>

<style>
	input {
		font-size: 130%;
		width: 7rem;
	}
	.input-group label {
		font-weight: bold;
		display: block;
	}
	.input-group .note {
		font-family: cursive;
		margin-top: 0;
	}
	.photos {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: flex-start;
		align-content: stretch;
		align-items: flex-start;
	}
	form,
	.photos,
	.explainer {
		background: white;
		margin: 1rem 2rem;
		border-radius: 2rem;
		padding: 1rem;
		/* max-width: 40rem; */
	}
	.photos .mini-example {
		max-width: 6rem;
	}
	.example-person {
		max-width: 10rem;
	}
	.photos {
		display: none;
	}
	@media screen and (min-width: 650px) {
		.wrapper {
			display: grid;
			grid-template-columns: 1fr 1fr;
			grid-template-rows: 1fr 1fr 1fr 1fr;
			gap: 0px 0px;
			grid-template-areas:
				'code form'
				'examples form'
				'examples form'
				'explainer explainer';
		}
		.the-url {
			grid-area: code;
		}
		form {
			grid-area: form;
		}
		.photos {
			grid-area: examples;
			display: initial;
		}
		.explainer {
			grid-area: explainer;
		}
		.input-group label {
			width: 6rem;
			display: inline-block;
			text-align: end;
		}
		.input-group .note {
			display: inline-block;
			max-width: 17rem;
		}
		.photos {
			display: flex;
			flex-direction: row;
			flex-wrap: wrap;
			justify-content: flex-start;
			align-content: stretch;
			align-items: flex-start;
		}
		form,
		.photos {
			background: white;
			margin: 1rem 2rem;
			border-radius: 2rem;
			padding: 1rem;
			/* max-width: 40rem; */
		}
		.the-url code {
			font-size: 200%;
		}
		.photos .mini-example {
			max-width: 10rem;
		}
		.example-person {
			max-width: 10rem;
		}
	}
</style>
