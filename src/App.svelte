<script>
  let responseData;
  let state;
  let features;

  async function getData() {
    try {
      const response = await fetch(
        `https://api.weather.gov/alerts/active?area=${state}`,
      );
      const data = await response.json();
      responseData = data;
      console.log(responseData);
      features = responseData.features;

      console.log(features);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }
</script>

<main>
  <!-- boxes will be used for multiple features during development-->
  <p>
    Enter a state by abbreviation to see weather alerts for that state in first
    box, example VA, KS, NY
  </p>
  <textarea name="1" id="box1" bind:value={state} cols="30" rows="2"
  ></textarea><br />

  <button on:click={getData}>Get Alerts</button>

  <br />
  <textarea name="1" id="box2" cols="30" rows="2"></textarea><br />

  <textarea name="1" id="box3" cols="30" rows="2"></textarea>
  <br />
  {#if features}
    {#each Object.entries(features) as [title, paragraph]}
      <h1>{title}</h1>
      <p>Location in {state}: {paragraph.properties.areaDesc}</p>

      <p>Certainty: {paragraph.properties.certainty}</p>

      <p>{paragraph.properties.description}</p>
    {/each}

    <!-- <pre>{JSON.stringify(features, null, 2)}</pre> -->
  {/if}
  <!-- display to screen

-->
</main>

<style>
</style>
