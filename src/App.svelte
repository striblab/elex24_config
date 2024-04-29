<script>
  import { onMount } from 'svelte';

  let district = '';
  let boundaries = '1';
  let isolate = '0';
  let interactive = '1';
  let shading = '0';
  let hed = '';
  let chatter = '';
  let showText = '0';
  let clicky = '1';
  let search = '1';
  let height = '600';
  let elections = []; // This will hold the fetched data

  onMount(async () => {
    const response = await fetch('./src/data/directory.json');
    elections = await response.json();
  });

  function handleSubmit(event) {
    // Prevent the default form submission behavior
    event.preventDefault();

    // Construct the URL with parameters
    const baseUrl = 'https://static.startribune.com/staging/news/projects/all/elex24maps';
    const params = new URLSearchParams({
      office: district,
      overlay: boundaries,
      filter: isolate,
      interactive,
      shading,
      height,
      text: showText,
      title: hed,
      chatter
    }).toString();

    let tag = `${baseUrl}?${params}`;

    document.getElementById("tagged").innerHTML = tag;

    // Open the new URL in a new window/tab
    window.open(tag, '_blank');
  }
</script>

<main id="main_body">
  <div id="form_container">
    <h1>StribLab Election Mapper 2024</h1>
    <form id="form_id" class="appnitro" on:submit={handleSubmit}>
      <div class="form_description">
        <h2>StribLab Election Mapper 2024</h2>
        <p>Embeddable Mapbox visualizations.</p>
      </div>

      <!-- Dropdown for selecting contest -->
      <div class="element">
        <label for="eOffice">Select Office:</label>
        <select bind:value={district} class="element select medium" id="eOffice">
          <option value="" selected></option>
          {#each elections as election}
            <option value={election.index}>{election.name}</option>
          {/each}
        </select>
      </div>

      <!-- Radio buttons for district borders -->
      <div class="element">
        <label>Boundaries:</label>
        <label>
          <input type="radio" bind:group={boundaries} value="1" checked> Enable
        </label>
        <label>
          <input type="radio" bind:group={boundaries} value="0"> Disable
        </label>
      </div>

      <!-- Radio buttons for zoom mode -->
      <div class="element">
        <label>Isolation Mode:</label>
        <label>
          <input type="radio" bind:group={isolate} value="0" checked> State
        </label>
        <label>
          <input type="radio" bind:group={isolate} value="1"> Metro
        </label>
      </div>

      <!-- Radio buttons for interactive controls -->
      <div class="element">
        <label>Interactive Mode:</label>
        <label>
          <input type="radio" bind:group={interactive} value="1" checked> Enable
        </label>
        <label>
          <input type="radio" bind:group={interactive} value="0"> Disable
        </label>
      </div>

      <!-- Radio buttons for map click interactivity -->
      <div class="element">
        <label>Click Interactivity:</label>
        <label>
          <input type="radio" bind:group={clicky} value="1" checked> Enable
        </label>
        <label>
          <input type="radio" bind:group={clicky} value="0"> Disable
        </label>
      </div>

      <!-- Radio buttons for map search -->
      <div class="element">
        <label>Search Feature:</label>
        <label>
          <input type="radio" bind:group={search} value="1" checked> Enable
        </label>
        <label>
          <input type="radio" bind:group={search} value="0"> Disable
        </label>
      </div>

      <!-- Radio buttons for result shading mode -->
      <div class="element">
        <label>Shading Mode:</label>
        <label>
          <input type="radio" bind:group={shading} value="0" checked> Partisan margin + vote density
        </label>
        <label>
          <input type="radio" bind:group={shading} value="1"> Classic partisan margin ramp
        </label>
      </div>

      <!-- Radio buttons for header text display -->
      <div class="element">
        <label>Header Text:</label>
        <label>
          <input type="radio" bind:group={showText} value="1"> Enable
        </label>
        <label>
          <input type="radio" bind:group={showText} value="0" checked> Disable
        </label>
      </div>

      <!-- Text inputs for data visualization headings and chatter -->
      <div class="element">
        <label for="eHed">Map Title:</label>
        <input type="text" bind:value={hed} class="element text medium" maxlength="255" id="eHed">
      </div>
      <div class="element">
        <label for="eChatter">Description:</label>
        <textarea bind:value={chatter} class="element textarea medium" id="eChatter"></textarea>
      </div>

      <!-- Text input for map height -->
      <div class="element">
        <label for="eHeight">Map Height:</label>
        <input type="text" bind:value={height} class="element text medium" maxlength="255" id="eHeight">
      </div>

      <!-- Submission button -->
      <button type="submit" class="button_text">GENERATE</button>
    </form>

    <div id="tagged"></div>
  </div>
</main>

<style>
  #form_container {
    max-width: 600px; /* Sets a max width for the form container */
    margin: 0 auto; /* Centers the form on the page */
    padding: 20px;
    background-color: #f4f4f4; /* Light grey background */
    border-radius: 8px; /* Rounded corners for the form container */
  }

  form {
    display: flex;
    flex-direction: column;
  }

  .element {
    margin-bottom: 20px;
    display: flex;
    align-items: center; /* Aligns items vertically */
    flex-wrap: wrap;
  }

  .element label {
    margin-right: 10px; /* Space between label and input */
    min-width: 120px; /* Ensures labels have enough space */
  }

  .element input[type="radio"] {
    margin-right: 5px;
  }

  .element.text, .element.textarea, .element.select {
    flex-grow: 1; /* Allows text inputs, textareas, and select to take available space */
  }

  .button_text {
    padding: 10px 20px;
    background-color: #007BFF; /* Blue background */
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .button_text:hover {
    background-color: #0056b3; /* Darker blue on hover */
  }
  #tagged {
    white-space:normal;
  }
</style>
