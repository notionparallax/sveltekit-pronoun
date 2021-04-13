<script>
	export let subject; // = 'He';
	export let object; // = 'Him';
	export let posessive; // = 'His';
	export let posessivePronoun; // = 'His';
	export let reflexive; // = 'Himself';
	export let emoji; // = '👫';
	export let colour; // = '#B00B55';
	export let height; // = '60';
	export let sep; // = '|';
	export let note; // = '';
	let cssClass;

	$: person = {
		subject,
		object,
		posessive,
		posessivePronoun,
		reflexive,
		emoji,
		colour,
		height,
		sep,
		note
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
</script>

<img class={cssClass} alt="A pronoun badge that reads {altText}" src={url} />

<style>
	img {
		max-width: 100%;
	}
</style>
