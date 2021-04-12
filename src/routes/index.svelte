<script>
	let person = {
	  subject: "He",
	  object: "Him",
	  posessive: "His",
	  posessivePronoun: "His",
	  reflexive: "Himself",
	  emoji: "👫",
	  colour: "B00B55",
	  height: "60",
	  sep: "|",
	  note: "",
	};
  
	$: altText = makeText(person);
	$: url = buildURL(person);
  
	function buildURL() {
	  let parts = [];
	  if (person.subject) parts.push(`subject=${person.subject}&`);
	  if (person.object) parts.push(`object=${person.object}&`);
	  if (person.posessive) parts.push(`posessive=${person.posessive}&`);
	  if (person.posessivePronoun)
		parts.push(`posessive-pronoun=${person.posessivePronoun}&`);
	  if (person.reflexive) parts.push(`reflexive=${person.reflexive}&`);
	  if (person.emoji) parts.push(`emoji=${person.emoji}&`);
	  if (person.colour) parts.push(`colour=${person.colour.replace("#", "")}&`);
	  if (person.height) parts.push(`height=${person.height}&`);
	  if (person.sep) parts.push(`sep=${person.sep}&`);
	  if (person.note) parts.push(`sep=${person.note}&`);
	  url = "https://pronoun.cyou/x/y?" + parts.join("");
	  if (url.slice(-1) == "&") {
		url = url.slice(0, -1);
	  }
	  return url;
	}
  
	function makeText() {
	  let parts = [];
	  if (person.subject) parts.push(person.subject);
	  if (person.object) parts.push(person.object);
	  if (person.posessive) parts.push(person.posessive);
	  if (person.posessivePronoun) parts.push(person.posessivePronoun);
	  if (person.reflexive) parts.push(person.reflexive);
	  if (person.emoji) parts.push(person.emoji); // this seems to break sapper
	  let text = parts.join(person.sep || "|");
  
	  return text;
	}
  
	import { onMount } from "svelte";
  
	let photos = [];
  
	onMount(async () => {
	  const res = await fetch(
		`https://jsonplaceholder.typicode.com/photos?_limit=4`
	  );
	  const theJson = await res.json();
	  const examplePeople = [
		{
		  subject: "Zie",
		  object: "Zim",
		  posessive: "Zis",
		  posessivePronoun: "Zir",
		  reflexive: "Zieself",
		},
		{
		  subject: "She",
		  object: "Her",
		  posessive: "Hers",
		  posessivePronoun: "Her",
		  reflexive: "Herself",
		},
		{
		  subject: "He",
		  object: "Him",
		  posessive: "His",
		  posessivePronoun: "His",
		  reflexive: "Himself",
		},
		{
		  subject: "Hep",
		  object: "Himp",
		  posessive: "His",
		  posessivePronoun: "His",
		  reflexive: "Himself",
		},
		{
		  subject: "1",
		  object: "2",
		  posessive: "3",
		  posessivePronoun: "3",
		  reflexive: "5",
		},
	  ];
  
	  theJson, examplePeople;
	  let temp = [];
	  theJson.forEach((e, i) => {
		temp.push({ ...e, ...examplePeople[i] });
	  });
	  console.log(temp);
	  photos = temp;
	});
  </script>
  
  

<svelte:head>
	<title>Pronoun Badge</title>
</svelte:head>

<div class="the-url">
  <a href={url} target="_blank"><code>{@html url.replace("&","<wbr>&")}</code></a>
  <p>Copy this text and paste it into <a href="https://github.com/notionparallax" title="this one is mine">your</a> <a href="https://docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme" title="here's how to make one of those">GitHub profile repo</a>, or wherever you can write html or markdown.</p>
  <figure>
    <img alt="A pronoun badge that reads {altText}" src={url} />
    <figcaption>This is the generated badge at that URL. Alt text: {altText}</figcaption>
  </figure>
</div>


<form>
  <p>Fill in this form with your pronouns. The code snippet above will update as 
    you go.<br>
    If you don't want to include a section, just leave it blank.</p>
  <div class="input-group">
    <label for="subject">subject</label>
    <input name="subject" bind:value={person.subject} />
    <p class="note">{person.subject} is amazing.</p>
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

  <!-- TODO: waiting for the server to accept it -->
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

<!-- <div class="photos">
  {#each photos as photo}
    <figure class="example-person">
      <img class="mini-example" alt="A pronoun badge that reads {altText}" src={url} /><br>
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
</div> -->

<style>
	.input-group label {
    width: 10rem;
    outline: 1px blue;
    display: inline-block;
    text-align: end;
	}
	.input-group .note {
		display: inline-block;
		max-width: 15rem;
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
  .the-url {
		background: white;
		margin: 1rem 2rem;
		border-radius: 2rem;
		padding: 1rem;
		max-width: 40rem;
	}
  .the-url code{
    font-size: 200%;
  }
  .photos .mini-example {
      max-width: 10rem;
  }
  .example-person{max-width: 10rem;}
</style>