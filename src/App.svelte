<script>
  let responseData;
  let state;
  let features;
  let longitude;
  let latitude;
  let weather2;
  let forecase2;
  let periods;

  async function weather() {
    try {
      const response = await fetch(
        `https://api.weather.gov/points/${latitude},${longitude}`,
      );
      const data = await response.json();
      responseData = data;

      console.log(responseData);
      weather2 = responseData.properties;
      let api = weather2.forecast;
      const response2 = await fetch(`${api}`);
      const data2 = await response2.json();

      console.log(data2);
      periods = data2.properties.periods;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }

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
  <p>enter latitude</p>
  <textarea name="1" id="box3" bind:value={latitude} cols="30" rows="2"
  ></textarea>
  <p>enter longitude</p>
  <textarea name="1" id="box2" bind:value={longitude} cols="30" rows="2"
  ></textarea><br />

  <br />

  <button on:click={weather}>Get Weather</button>

  {#if periods}
    {#each Object.entries(periods) as [title, paragraph]}
      <h1>{title}</h1>
      <p>Time: {paragraph.name}</p>

      <p>When: {paragraph.detailedForecast}</p>
      <p>Temperature: {paragraph.temperature}</p>
    {/each}

    <!-- <pre>{JSON.stringify(features, null, 2)}</pre> -->
  {/if}

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
