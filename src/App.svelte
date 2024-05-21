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
    const baseUrl = 'https://static.startribune.com/staging/news/projects/all/elex24maps/index.html';
    const params = new URLSearchParams({
      office: district,
      overlay: boundaries,
      filter: isolate,
      interactive,
      shading,
      search,
      clicky,
      height,
      text: showText,
      title: hed,
      chatter
    }).toString();

    let tag = `${baseUrl}?${params}`;

    //document.getElementById("tagged").innerHTML = tag;

    // Open the new URL in a new window/tab
    window.open(tag, '_blank');
  }
</script>

<main id="main_body">
  <div id="form_container">
    <h1>StribLab Election 2022 Precinct Mapper</h1>
    <form id="form_id" class="appnitro" on:submit={handleSubmit}>
      <div class="form_description">
        <h2>Configure and generate precinct result maps</h2>
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
        <div class="options">
          <label class="control control-radio">
            <input type="radio" id="boundaryEnable" bind:group={boundaries} value="1" checked>
            <div class="control_indicator"></div>
            Enable
          </label>
          <label class="control control-radio">
            <input type="radio" id="boundaryDisable" bind:group={boundaries} value="0">
            <div class="control_indicator"></div>
            Disable
          </label>
        </div>
      </div>

      <!-- Radio buttons for zoom mode -->
      <div class="element">
        <label>Zoom Mode:</label>
        <div class="options">
          <label class="control control-radio">
            <input type="radio" id="zoomState" bind:group={isolate} value="0" checked>
            <div class="control_indicator"></div>
            State
          </label>
          <label class="control control-radio">
            <input type="radio" id="zoomMetro" bind:group={isolate} value="1">
            <div class="control_indicator"></div>
            Metro
          </label>
        </div>
      </div>

      <!-- Radio buttons for interactive controls -->
      <div class="element">
        <label>Interactivity</label>
        <div class="options">
          <label class="control control-radio">
            <input type="radio" id="interactiveYes" bind:group={interactive} value="1" checked>
            <div class="control_indicator"></div>
            Enable
          </label>
          <label class="control control-radio">
            <input type="radio" id="interactiveNo" bind:group={interactive} value="0">
            <div class="control_indicator"></div>
            Disable
          </label>
        </div>
      </div>

      <!-- Radio buttons for map click interactivity -->
      <div class="element">
        <label>Controls and Tooltips:</label>
        <div class="options">
          <label class="control control-radio">
            <input type="radio" id="clickEnable" bind:group={clicky} value="1" checked>
            <div class="control_indicator"></div>
            Enable
          </label>
          <label class="control control-radio">
            <input type="radio" id="clickDisable" bind:group={clicky} value="0">
            <div class="control_indicator"></div>
            Disable
          </label>
        </div>
      </div>

      <!-- Radio buttons for map search -->
      <div class="element">
        <label>Search Feature:</label>
        <div class="options">
          <label class="control control-radio">
            <input type="radio" id="searchEnable" bind:group={search} value="1" checked>
            <div class="control_indicator"></div>
            Enable
          </label>
          <label class="control control-radio">
            <input type="radio" id="searchDisable" bind:group={search} value="0">
            <div class="control_indicator"></div>
            Disable
          </label>
        </div>
      </div>

      <div class="element">
        <label>Header Text:</label>
          <div class="options">
            <label class="control control-radio">
              <input type="radio" id="headerYes" bind:group={showText} value="1" >
              <div class="control_indicator"></div>
              Enable
            </label>
            <label class="control control-radio">
              <input type="radio" id="headerNo" bind:group={showText} value="0" checked>
              <div class="control_indicator"></div>
              Disable
            </label>
          </div>
      </div>


      <!-- Text inputs for data visualization headings and chatter -->
      <div class="element">
        <label for="eHed">Map Title:</label>
      </div>
      <input type="text" bind:value={hed} class="element text medium" maxlength="255" id="eHed">
      <div class="element">
        <label for="eChatter">Description:</label>
        <textarea bind:value={chatter} class="element textarea medium" id="eChatter"></textarea>
      </div>

      <!-- Text input for map height -->
      <div class="element">
        <label for="eHeight">Map Height:</label>
      </div>
      <input type="text" bind:value={height} class="element text medium" maxlength="255" id="eHeight">

      <!-- Submission button -->
      <button type="submit" class="button_text">GENERATE</button>
    </form>
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

  .disclaimer {
    color:#de2d26;
  }

  .setting {
    width:80%;
    display:inline-block;
  }

  .control {
    font-family: arial;
    display: block;
    position: relative;
    padding-left: 30px;
    margin-bottom: 5px;
    padding-top: 3px;
    cursor: pointer;
    font-size: 16px;
  } 
  
    .control input {
        position: absolute;
        z-index: -1;
        opacity: 0;
    }
    
    .control_indicator {
        position: absolute;
        top: 2px;
        left: 0;
        height: 20px;
        width: 20px;
        background: #e6e6e6;
        border: 0px solid #000000;
        border-radius: undefinedpx;
    }
    .control:hover input ~ .control_indicator,
    .control input:focus ~ .control_indicator {
        background: #cccccc;
    }

    .control input:checked ~ .control_indicator {
        background: #1e7042;
    }
    .control:hover input:not([disabled]):checked ~ .control_indicator,
    .control input:checked:focus ~ .control_indicator {
        background: #0e6647d;
    }
    .control input:disabled ~ .control_indicator {
        background: #e6e6e6;
        opacity: 0.6;
        pointer-events: none;
    }
    .control_indicator:after {
        box-sizing: unset;
        content: '';
        position: absolute;
        display: none;
    }
    .control input:checked ~ .control_indicator:after {
        display: block;
    }
    .control-radio .control_indicator {
        border-radius: 50%;
    }

    .control-radio .control_indicator:after {
        left: 7px;
        top: 7px;
        height: 6px;
        width: 6px;
        border-radius: 50%;
        background: #ffffff;
        transition: background 250ms;
    }
    .control-radio input:disabled ~ .control_indicator:after {
        background: #7b7b7b;
    }.control-radio .control_indicator::before {
        content: '';
        display: block;
        position: absolute;
        left: 0;
        top: 0;
        width: 4.5rem;
        height: 4.5rem;
        margin-left: -1.3rem;
        margin-top: -1.3rem;
        background: #2aa1c0;
        border-radius: 3rem;
        opacity: 0.6;
        z-index: 99999;
        transform: scale(0);
    }
    @keyframes s-ripple {
        0% {
            opacity: 0;
            transform: scale(0);
        }
        20% {
            transform: scale(1);
        }
        100% {
            opacity: 0.01;
            transform: scale(1);
        }
    }
    @keyframes s-ripple-dup {
      0% {
          transform: scale(0);
        }
      30% {
            transform: scale(1);
        }
        60% {
            transform: scale(1);
        }
        100% {
            opacity: 0;
            transform: scale(1);
        }
    }
    .control-radio input + .control_indicator::before {
        animation: s-ripple 250ms ease-out;
    }
    .control-radio input:checked + .control_indicator::before {
        animation-name: s-ripple-dup;
    }
  
</style>
