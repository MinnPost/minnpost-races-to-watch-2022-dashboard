<style>
	.candidate {
		border: 1px solid #d6d6da;
		padding: 0.5em 0.25em;
		border-radius: 4px;
		display: block;
		width: 100%;
		margin-right: 0.25em;
		margin-bottom: 0.5em;
	}

	@media screen and (min-width: 30em) {
		.candidate {
			width: 48.275862069%;
		}
	}

	@media screen and (min-width: 30em) and (max-width: 50em) {
		.candidate:nth-child(2n) {
			margin-right: 0;
		}
	}

	@media screen and (min-width: 50em) {
		.candidate {
			width: 32.2033898305%;
		}
		.candidate:nth-child(3n) {
			margin-right: 0;
		}
	}

	.m-entry-excerpt {
		font-family: "ff-meta-serif-web-pro", georgia, cambria, "Times New Roman", times, serif;
	}

	.former-candidate {
		opacity: 0.6;
	}

	.candidate-photo {
		float: right;
	}

	.candidate-meta {
		font-weight: 700;
		margin-bottom: 0.25em;
		font-size: var(--scale-3);
		font-family: "ff-meta-web-pro", helvetica, arial, sans-serif;
		color: #5E6E76;
		width: 100%;
	}

	.candidate-meta div {
		margin-bottom: 0.25em;
	}

	.incumbent {color: #f74607;}
	.first-elected {
		color: #5E6E76;
		padding-left:1.5em;
		font-weight: 400;
	}
	.deg45 {
		transform: rotate(45deg);
		font-size: .7em;
	}
	
</style>

<script>
	// a single candidate record from Race.svelte
	export let candidate;

	const party_icons = {
		"republican": "republican",
		"dfl": "democrat",
		"legal-marijuana-now": "cannabis",
		"grassroots-legalize-cannabis": "cannabis",
		"green": "square deg45"
	};

	function capString(s) {
		return s.charAt(0).toUpperCase() + s.slice(1);
	}


</script>

<article class="m-post candidate" class:former-candidate={candidate["dropped-out"]}>
	
	<h5 class="a-entry-title">{candidate.name}</h5>

	<div class="m-entry-meta candidate-meta">
		{#if candidate["party"]}
			<div class="party-name party-{candidate["party-id"]}"><i class="fas fa-fw fa-{party_icons[candidate["party-id"]] ?? "circle"}"></i> {candidate.party}</div>
		{/if}

		{#if candidate["lives-in"]}
			<div class="hometown"><i class="fas fa-fw fa-home"></i> Lives in: {candidate["lives-in"]}</div>
		{/if}

		{#if candidate["incumbent"]}
		<div class="incumbent">
			<div class="incumbent"><i class="fas fa-fw fa-star"></i> Member of {capString(candidate.chamber)}</div>
			{#if candidate["year-first-elected"]}
			<div class="first-elected">
				First elected in {candidate["year-first-elected"]}
			</div>
			{/if}
		</div>
		{/if}

		{#if candidate["endorsed"]}
			<div class="endorsed">
				<span class="icon party-{candidate["party-id"]}"><i class="fas fa-fw fa-check-square"></i></span> 
				Endorsed by <span class="party-{candidate["party-id"]}">{#if candidate.party == "Republican"}GOP{:else}{candidate.party}{/if}</span>
			</div>
		{/if}

		{#if candidate.website}
			<div class="website"><a href="{candidate.website}" target="_blank"><i class="fas fa-fw fa-globe"></i> Campaign website</a></div>
		{/if}

	</div>
	

</article>
