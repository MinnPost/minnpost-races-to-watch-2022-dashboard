<style>
	/*
	23.7288135593%
	32.2033898305%
	48.275862069%
	*/
	:global(.district-listing) {
		border-bottom: 1px solid #d6d6da;
		margin-bottom: 1.5em;
		padding-bottom: .75em;
	}
	:global(.district-listing:last-of-type) {
		border-bottom: none;
		margin-bottom: 0;
		padding-bottom: 0;
	}
	:global(.candidates-listing) {
    	display: flex;
    	flex-wrap: wrap;
		margin-top: 0.5em;
		margin-bottom: 1.5em;
		justify-content: flex-start;
		
  	}
	:global(.candidates-listing h3, .candidates-listing h4) {
		width: 100%;
		/*margin-bottom: 0.25em;*/
	}

	:global(.m-homepage-zones .m-zone) {
		max-width: none;
		width: 100%;
	}

	:global(.party-name) {
		color: #869298;
	}

	:global(.party-name:before) {
		border-radius: 50%;
		align-items: center;
		color: #fff;
		background-color: #869298;
		font-weight: 700;
		margin-right: 0.25em;		
		height: 1.25em;
		width: 1.25em;
		display: inline-flex;
		justify-content: center;
		flex: 0 0 auto;
	}

	:global(.party-dfl) {color: #0793AB;}
	:global(.party-dfl-icon:before) {
		content: "D";
		background: #0793AB;
	}

	:global(.party-republican) {color: #A1000F}
	:global(.party-republican-icon:before) {
		content: "R";
		background: #A1000F;
	}

	:global(.party-legal-marijuana-now) {color: #286806;}
	:global(.party-legal-marijuana-icon:before) {
		content: "L";
		background: #286806;
	}

	:global(.party-grassroots-legalize-cannabis) {color: #41AB07;}
	:global(.party-grassroots-legalize-cannabis-icon:before) {
		content: "G";
		background: #41AB07;
	}

	:global(.party-green) {color: #07AB20;}
	:global(.party-green-icon:before) {
		content: "G";
		background: #07AB20;
	}

    :global(.m-chamber-header) {text-transform: capitalize;}

	
</style>

<script>	
	import AllCandidates from './AllCandidates.svelte'

	// remote data
	let items = []
	async function getData() {
		/*
		for actual 2022 data:
		https://s3.amazonaws.com/data.minnpost/projects/spreadsheets/1qrJaQEerdDI2eghz8dYbxB1eAhjw7rYhDOrqapHqR_Y-Candidates|Races-custom.json
		for local data (if flask app is running):
		http://127.0.0.1:5001/candidate-tracker/json/
		*/
		let res = await fetch(`http://127.0.0.1:5001/candidate-tracker/json/`);
		let data = await res.json();
		items = data
		return items;
	}
    const dataPromise = getData();

	// create the promise result
	$: filteredList = dataPromise.then((r) => {
		// filter the districts and/or candidates by the search term
		let candidates = items.candidates;
		let races = items.races;

		// filter chambers
		let chambers = items.races.reduce(function(filtered, item) {
			if ( item.chamber ) {
				let chamber = item.chamber;
				filtered.push(chamber);
			}
			return [...new Set(filtered)];
		}, []);

		// make the final data array for filteredList to use and return it
		let data = [];

		if ( typeof races !== "undefined" ) {
			data["races"] = races;
		}
		if ( typeof chambers !== "undefined" ) {
			data["chambers"] = chambers;
		}
		if ( typeof candidates !== "undefined" ) {
			data["candidates"] = candidates;
		}

		return data;
	});
	
</script>


<section class="container m-zones m-homepage-zones">
	{#await filteredList}
		Loading...
	{:then items}
		<svelte:component this={AllCandidates} items="{items}" />
	{/await}
</section>
