<script>
  import { onMount } from 'svelte';

  let district = '';
  let boundaries = '1';
  let isolate = '0';
  let interactive = '1';
  let shading = '0';
  let height = '600';
  let hed = '';
  let chatter = '';
  let showtext = '0';
  let clicky = '1';
  let search = '1';
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
      title: hed,
      chatter,
      height,
      text: showtext,
      clicky,
      search
    }).toString();

    let tag = `${baseUrl}?${params}`;

    //document.getElementById("tagged").innerHTML = tag;

    // Open the new URL in a new window/tab
    window.open(tag, '_blank');
  }
</script>

<main id="main_body">
  <div id="form_container">
    <form id="form_id" class="appnitro" on:submit={handleSubmit}>
      <div class="form_description">
        <h2>StribLab Election 2024 Mapifier</h2>
        <p>Configure and generate precinct-based election result maps.</p>
      </div>

      <div class="element">
        <label for="eOffice">Select Office:</label>
        <select bind:value={district} class="element select medium" id="eOffice">
          <option value="" selected></option>
          {#each elections as election}
            <option value={election.index}>{election.name}</option>
          {/each}
        </select>
      </div>

      <ul>
      <li>
        <label class="description" for="eDistricts">District borders</label>
        <span>
            <label class="control control-radio">
                <input id="eDistricts_1" name="eDistricts" class="element radio" type="radio" value="1" checked bind:group={boundaries}  />
                <div class="control_indicator"></div>
                Enable
            </label>
            <label class="control control-radio">
                <input id="eDistricts_2" name="eDistricts" class="element radio" type="radio" value="0" bind:group={boundaries} />
                <div class="control_indicator"></div>
                Disable
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eIsolate">Zoom mode</label>
        <span>
            <label class="control control-radio">
                <input id="eIsolate_1" name="eIsolate" class="element radio" type="radio" value="0" checked bind:group={isolate} />
                <div class="control_indicator"></div>
                State
            </label>
            <label class="control control-radio">
                <input id="eIsolate_2" name="eIsolate" class="element radio" type="radio" value="1" bind:group={isolate} />
                <div class="control_indicator"></div>
                Metro
            </label>
            <label class="control control-radio">
                <input id="eIsolate_3" name="eIsolate" class="element radio" type="radio" value="2" bind:group={isolate} />
                <div class="control_indicator"></div>
                Isolate districts
            </label>
            <label class="control control-radio">
                <input id="eIsolate_4" name="eIsolate" class="element radio" type="radio" value="3" bind:group={isolate} />
                <div class="control_indicator"></div>
                Large map
            </label>
            <label class="control control-radio">
                <input id="eIsolate_5" name="eIsolate" class="element radio" type="radio" value="4" bind:group={isolate} />
                <div class="control_indicator"></div>
                Hennepin County
            </label>
            <label class="control control-radio">
                <input id="eIsolate_6" name="eIsolate" class="element radio" type="radio" value="5" bind:group={isolate} />
                <div class="control_indicator"></div>
                Duluth
            </label>
            <label class="control control-radio">
                <input id="eIsolate_7" name="eIsolate" class="element radio" type="radio" value="6" bind:group={isolate} />
                <div class="control_indicator"></div>
                St. Cloud
            </label>
            <label class="control control-radio">
                <input id="eIsolate_8" name="eIsolate" class="element radio" type="radio" value="7" bind:group={isolate} />
                <div class="control_indicator"></div>
                Moorhead
            </label>
            <label class="control control-radio">
                <input id="eIsolate_9" name="eIsolate" class="element radio" type="radio" value="8" bind:group={isolate} />
                <div class="control_indicator"></div>
                Mankato
            </label>
            <label class="control control-radio">
                <input id="eIsolate_10" name="eIsolate" class="element radio" type="radio" value="9" bind:group={isolate} />
                <div class="control_indicator"></div>
                Rochester
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eInteract">State/metro controls</label>
        <span>
            <label class="control control-radio">
                <input id="eInteract_1" name="eInteract" class="element radio" type="radio" value="1" checked bind:group={interactive} />
                <div class="control_indicator"></div>
                Enable
            </label>
            <label class="control control-radio">
                <input id="eInteract_2" name="eInteract" class="element radio" type="radio" value="0" bind:group={interactive} />
                <div class="control_indicator"></div>
                Disable
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eClicky">Map interactivity</label>
        <span>
            <label class="control control-radio">
                <input id="eClicky_1" name="eClicky" class="element radio" type="radio" value="1" checked bind:group={clicky} />
                <div class="control_indicator"></div>
                Enable
            </label>
            <label class="control control-radio">
                <input id="eClicky_2" name="eClicky" class="element radio" type="radio" value="0" bind:group={clicky}  />
                <div class="control_indicator"></div>
                Disable
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eSearch">Map search</label>
        <span>
            <label class="control control-radio">
                <input id="eSearch_1" name="eSearch" class="element radio" type="radio" value="1" checked bind:group={search} />
                <div class="control_indicator"></div>
                Enable
            </label>
            <label class="control control-radio">
                <input id="eSearch_2" name="eSearch" class="element radio" type="radio" value="0" bind:group={search} />
                <div class="control_indicator"></div>
                Disable
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eShading">Result shading mode</label>
        <span>
            <label class="control control-radio">
                <input id="eShading_1" name="eShading" class="element radio" type="radio" value="0" checked bind:group={shading} />
                <div class="control_indicator"></div>
                Partisan margin + vote density
            </label>
            <label class="control control-radio">
                <input id="eShading_2" name="eShading" class="element radio" type="radio" value="1" bind:group={shading} />
                <div class="control_indicator"></div>
                Classic partisan margin ramp
            </label>
            <label class="control control-radio">
                <input id="eShading_3" name="eShading" class="element radio" type="radio" value="2" bind:group={shading} />
                <div class="control_indicator"></div>
                Partisan binary [Red vs. Blue]
            </label>
            <label class="control control-radio">
                <input id="eShading_3" name="eShading" class="element radio" type="radio" value="3" bind:group={shading} />
                <div class="control_indicator"></div>
                Party primary [Orange vs. Green]
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eHeader">Show header text</label>
        <span>
            <label class="control control-radio">
                <input id="eText_1" name="eText" class="element radio" type="radio" value="1" bind:group={showtext} />
                <div class="control_indicator"></div>
                Enable
            </label>
            <label class="control control-radio">
                <input id="eText_2" name="eText" class="element radio" type="radio" value="0" checked bind:group={showtext}  />
                <div class="control_indicator"></div>
                Disable
            </label>
        </span> 
    </li>
    <li>
        <label class="description" for="eHed">Dataviz hed text</label>
        <div>
            <input id="eHed" name="eHed" class="element text medium" type="text" maxlength="255" bind:value={hed} /> 
        </div> 
    </li>
    <li>
        <label class="description" for="eChatter">Dataviz chatter text</label>
        <div>
            <textarea id="eChatter" name="eChatter" class="element textarea medium" bind:value={chatter}></textarea> 
        </div> 
    </li>
    <li>
        <label class="description" for="eHeight">Map height in px</label>
        <div>
            <input id="eHeight" name="eHeight" class="element text medium" type="text" maxlength="255" bind:value={height} /> 
        </div> 
    </li>
    <li class="buttons">
        <input type="hidden" name="form_id" value="42838" />
        <button type="submit" class="button_text">GENERATE</button>
    </li>
  </ul>
    </form>

<hr />
<div id="downloads">
<h2>Results file downloads</h2>
<p class="dlabel">Presidential</p>
<ul>
    <li>POTUS results by precinct</li>
    <div><a href="../store/potus-results-geo.json">GEOJSON</a> | <a href="../store/potus-results-geo.csv">CSV</a></div>
    <li>POTUS results by county</li>
    <div><a href="../store/potus-results-county.json">GEOJSON</a> | <a href="../store/potus-results-county.csv">CSV</a></div>
    <li>POTUS results by congressional district</li>
    <div><a href="../store/potus-results-district.json">GEOJSON</a> | <a href="../store/potus-results-district.csv">CSV</a></div>
    <li>POTUS results by legislative district</li>
    <div><a href="../store/potus-results-leg.json">GEOJSON</a> | <a href="../store/potus-results-leg.csv">CSV</a></div>
</ul>

<p class="dlabel">Senatorial</p>
<ul>
    <li>Senate results by precinct</li>
    <div><a href="../store/senate-results-geo.json">GEOJSON</a> | <a href="../store/senate-results-geo.csv">CSV</a></div>
    <li>Senate results by county</li>
    <div><a href="../store/senate-results-county.json">GEOJSON</a> | <a href="../store/senate-results-county.csv">CSV</a></div>
    <li>Senate results by congressional district</li>
    <div><a href="../store/senate-results-district.json">GEOJSON</a> | <a href="../store/senate-results-district.csv">CSV</a></div>
</ul>


<p class="dlabel">Congressional</p>
<ul>
    <li>U.S. House results by precinct</li>
    <div><a href="../store/congress-results-geo.json">GEOJSON</a> | <a href="../store/congress-results-geo.csv">CSV</a></div>
    <li>U.S. House results by congressional district</li>
    <div><a href="../store/congress-results-district.json">GEOJSON</a> | <a href="../store/congress-results-district.csv">CSV</a></div>
</ul>


<p class="dlabel">Legislative</p>
<ul>
    <li>MNLEG results by precinct</li>
    <div><a href="../store/mnleg-results-geo.json">GEOJSON</a> | <a href="../store/mnleg-results-geo.csv">CSV</a></div>
    <li>MNLEG by legislative district</li>
    <div><a href="../store/mnleg-results-leg.json">GEOJSON</a> | <a href="../store/mnleg-results-leg.csv">CSV</a></div>
</ul>
</div>
</main>




<style>
.dlabel {
    margin-top:18px;
    font-weight:bold
}
#downloads li {
    font-family: Arial, Helvetica, sans-serif;
}

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
        height: 2px;
        width: 10px;
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
        left: 2px;
        top: 2px;
        height: 6px;
        width: 6px;
        border-radius: 50%;
        background: #ffffff;
        transition: background 250ms;
    }
    .control-radio input:disabled ~ .control_indicator:after {
        background: #7b7b7b;
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
