<script>
	export let url = '';
	export let altText = "alt text isn't working for some reason";
	export let person;

	import Badge from './_Badge.svelte';

	const stringToClipboard = (str) => {
		const el = document.createElement('textarea');
		el.value = str;
		document.body.appendChild(el);
		el.select();
		document.execCommand('copy');
		document.body.removeChild(el);
	};
  const copyToClipboard = () =>{
    const t = document.getElementById("the-actual-url").innerText;
    stringToClipboard(t)
  }
</script>

<div class="the-url">
	<!-- <a class="display-url" href={url} target="_blank"> -->
	<code on:click={copyToClipboard}>
    <span id="the-actual-url">{@html url.replace(/(\/+|\?|\&)/gm, `$1<wbr>`)}</span>
    <span class="copy-note">Click to copy this to your clipboard 📄</span>
  </code>
	<!-- </a> -->
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
    display: inline-block;
    border-radius: 0.5em;
    padding: 0.5em;
    background: rgb(255 255 255 / 50%);
    position: relative;
  }
  img {
    max-width: 100%;
  }
  .copy-note {
    font-size: 40%;
    display: block;
    position: absolute;
    right: 0.3rem;
    opacity: 0.5;
    bottom: 0.3rem;
  }
  .copy-note:hover {opacity: 1;}
  @media screen and (min-width: 650px) {
    .the-url code {
      font-size: 200%;
    }
    figure {
      margin: 0;
    }
	}
</style>
