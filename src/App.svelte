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
    <form id="form_id" class="appnitro" on:submit={handleSubmit}>
      <div class="form_description">
        <h2>StribLab Election Mapper 2024</h2>
        <p></p>
      </div>

      <!-- Dropdown for selecting contest -->
      <p for="eOffice">Select Office</p>
      <div class="element">
        <select bind:value={district} class="element select medium" id="eOffice">
          <option value="" selected></option>
          {#each elections as election}
            <option value={election.index}>{election.name}</option>
          {/each}
        </select>
      </div>
      
      <!-- Radio buttons for district borders -->
      <p>Boundaries</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="boundariesYes" bind:group={boundaries} value="1" checked>
          <label for="boundariesYes">Enable</label>
          <input type="radio" id="boundariesNo" bind:group={boundaries} value="0">
          <label for="boundariesNo">Disable</label>
        </div>
      </div>

      <!-- Radio buttons for zoom mode -->
      <p>Isolation Mode</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="isolateState" bind:group={isolate} value="0" checked>
          <label for="isolateState">State</label>
          <input type="radio" id="isolateMetro" bind:group={isolate} value="1">
          <label for="isolateMetro">Metro</label>
        </div>
      </div>

      <p>Interactive Mode</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="interactiveYes" bind:group={interactive} value="1" checked>
          <label for="interactiveYes">Enable</label>
          <input type="radio" id="interactiveNo" bind:group={interactive} value="0">
          <label for="interactiveNo">Disable</label>
        </div>
      </div>
      
      <p>Click Interactivity</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="clickyYes" bind:group={clicky} value="1" checked>
          <label for="clickyYes">Enable</label>
          <input type="radio" id="clickyNo" bind:group={clicky} value="0">
          <label for="clickyNo">Disable</label>
        </div>
      </div>
      
      <p>Search Feature</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="searchYes" bind:group={search} value="1" checked>
          <label for="searchYes">Enable</label>
          <input type="radio" id="searchNo" bind:group={search} value="0">
          <label for="searchNo">Disable</label>
        </div>
      </div>
      
      <p>Shading Mode</p>
      <div class="element">
        <div class="options">
          <input type="radio" id="shadingYes" bind:group={shading} value="1" checked>
          <label for="shadingYes">
            Partisan margin + vote density
            </label>
          <input type="radio" id="shadingNo" bind:group={shading} value="0">
          <label for="shadingNo">Classic partisan margin ramp</label>
        </div>
      </div>

      <!-- Radio buttons for header text display -->
      <p>Header Text</p>
      <div class="element">
          <input type="radio" id="headerYes" bind:group={showText} value="1"> 
          <label for="headerYes">Enable</label>
          <input type="radio" id="headerNo" bind:group={showText} value="0" checked>
          <label for="headerNo">Disable</label>
      </div>

      <!-- Text inputs for data visualization headings and chatter -->
      <p>Map Title</p>
      <div class="element">
        <input type="text" bind:value={hed} class="element text medium" maxlength="255" id="eHed">
      </div>

      <p>Description</p>
      <div class="element">
        <textarea bind:value={chatter} class="element textarea medium" id="eChatter"></textarea>
      </div>

      <!-- Text input for map height -->
      <p>Map Height</p>
      <div class="element">
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
  }

  .options {
    display: flex;
    align-items: center; /* Ensures radio buttons and labels are aligned */
  }

  .options label {
    margin-right: 20px; /* Space after each label */
  }

  input[type="radio"] {
    margin-right: 5px; /* Space before the label */
  }

  .button_text {
    padding: 10px 20px;
    background-color: #1e7042; /* Blue background */
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .button_text:hover {
    background-color: #1e7042; /* Darker blue on hover */
  }
</style>
