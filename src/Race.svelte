<script>

export let office;
export let candidates;

// single candidate template
import Candidate from "./Candidate.svelte";
import Map from "./components/Map.svelte";

let showMap = false;

const party_icons = {
  "republican": "republican",
  "dfl": "democrat",
  "legal-marijuana-now": "cannabis",
  "grassroots-legalize-cannabis": "cannabis",
  "green": "square deg45"
};

</script>

<style>
  .map-container {
    height: 200px;
    margin-top: 0.25em;
    padding-bottom: 0.75em;
  }

  h3 {
    margin-bottom: 0.25em;
  }

  h4 {
    margin-bottom: 0.25em;
  }

  h5 {
    font-weight: 400;
    font-style: italic;
    color: #5E6E76;
    display: inline-block;
  }

  .m-district {
    margin-bottom: .75em;
  }

  .district-info {
    margin-bottom: 0.75em;
  }

  .district-info .past-elections div {
    display: inline-block;
  }
  .district-info .past-elections li {
    margin-bottom: 0.25em;
  }

  button {
    background-color: #ffffff;
    border: 1px solid #D6D6DA;
    border-radius: 5px;
    color: #5E6E76;
    font-size: 0.6em;
    vertical-align: middle;
    margin-left: 1em;
  }

</style>

<section class="m-district">
  <h3>{office.office}</h3>
  <h5>{office.region}</h5>
  {#if showMap}
    <button value="" on:click="{() => showMap = false}">Hide map</button>
    <div class="map-container">
      <Map district={office.district}/>
    </div>
  {:else}
  <button value="" on:click="{() => showMap = true}">Show on map</button>
  {/if}
  <div class="district-info">
    {#if office.blurb}
        <p>{@html office.blurb}</p>
    {/if}
    <h4>Past election results in this district</h4>
    {#if office["2020-pres"] || office["2016-pres"] || office["2016-20-comp"] }
      <ul class="past-elections">
        <li>2020 presidential election:
          <div class="party-name party-{office["2020-pres-party-id"]}">
            <i class="fas fa-fw fa-{party_icons[office["2020-pres-party-id"]] ?? "circle"}"></i>
            {office['2020-pres']}
          </div>
        </li>
        <li>2016 presidential election:
          <div class="party-name party-{office["2016-pres-party-id"]}">
            <i class="fas fa-fw fa-{party_icons[office["2016-pres-party-id"]] ?? "circle"}"></i>
            {office['2016-pres']}
          </div>
        </li>
        <li>2016 through 2020 elections composite:
          <div class="party-name party-{office["2016-20-party-id"]}">
            <i class="fas fa-fw fa-{party_icons[office["2016-20-party-id"]] ?? "circle"}"></i>
            {office['2016-20-comp']}
          </div>
        </li>
      </ul>
    {/if}
  </div>
  <div class="candidates-listing">
  {#each candidates as candidate}
    <Candidate candidate = {candidate} />
  {/each}
  </div>
</section>
