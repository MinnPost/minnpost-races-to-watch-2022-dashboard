<script>
    // all data for candidates and races
    export let items;

    // the candidates from App.svelte
	let candidates = items.candidates;

    // the races from App.svelte
    let races = items.races;

    // create a list of races in their chamber
	let chamber_offices = function(chamber) {
        races = items.races.filter(
            item => item["chamber"] == chamber
        );
        return races;
	}

    // create a list of candidates in their chamber/office
	let office_candidates = function(chamber, office = '') {
        if (office !== '') {
            candidates = items.candidates.filter(
                item => item["chamber"] == chamber && item["office-sought"] == office
            );
        } else {
            candidates = items.candidates.filter(
                item => item["chamber"] == chamber
            );
        }
        return candidates;
	}

    // single candidate template
	import Race from "./Race.svelte";
    
</script>

{#each items.chambers as chamber}
    <section class="chamber-listing">
        <h2 class="m-archive-header m-chamber-header">{chamber}</h2>
        {#each chamber_offices(chamber) as office}
            <Race office={office} candidates={office_candidates(chamber, office.office)} />
        {/each}
    </section>
{/each}
