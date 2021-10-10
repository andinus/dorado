<svelte:head>
  <script src="./plotly/plotly.min.js" on:load={plotlyLoaded}></script>
</svelte:head>
<script>
  let solved = false;

  let plotlyReady = false;
  const plotlyLoaded = () => {
    plotlyReady = true;
    dda();
  }

  let x0 = 0, y0 = 0, x1 = 4, y1 = 4;
  let dx, dy;

  let steps;
  let x_inc, y_inc;

  let x_axis = [], y_axis = [];
  let x_axis_floored = [], y_axis_floored = [];

  function dda() {
    dx = Math.abs(x1 - x0);
    dy = Math.abs(y1 - y0);

    steps = Math.max(...[dx, dy].map(x => Math.abs(x)));

    x_inc = dx / steps;
    y_inc = dy / steps;

    // Build the x axis.
    x_axis = [];
    let X = x0;
    for (let idx = 0; idx < steps; idx++) {
      X += x_inc;
      x_axis.push(X.toPrecision(3));
    }
    x_axis_floored = x_axis.map(function(x) {return Math.round(x)});

    // Build the y axis.
    y_axis = [];
    let Y = y0;
    for (let idx = 0; idx < steps; idx++) {
      Y += y_inc;
      y_axis.push(Y.toPrecision(3));
    }
    y_axis_floored = y_axis.map(function(y) {return Math.round(y)});

    // Plot the chart.
    if (plotlyReady)
      Plotly.newPlot('algoChart', [{
        x: x_axis_floored,
        y: y_axis_floored,
        type: 'lines',
        name: 'DDA'
      }]);

    solved = true;
  }
</script>

<h2>Digital differential analyzer</h2>
<form class="points">
  <label for="x0">x0: </label>
  <input type="number" id="x0" name="x0" on:change={dda} bind:value={x0}>&nbsp;
  <label for="y0">y0: </label>
  <input type="number" id="y0" name="y0" on:change={dda} bind:value={y0}>
  <br>
  <label for="x1">x1: </label>
  <input type="number" id="x1" name="x1" on:change={dda} bind:value={x1}>&nbsp;
  <label for="y1">y1: </label>
  <input type="number" id="y1" name="y1" on:change={dda} bind:value={y1}>
  <br>
  <button type="button" on:click={dda}>Solve</button>
</form>

{#if solved === true && steps > 0}
  <hr>
  <h3>Solution</h3>

  <label for="dx">dx = <code>|x1 - x0|</code>: </label>
  <input type="text" id="dx" name="dx" value={dx} disabled><br>
  <label for="dy">dy = <code>|y1 - y0|</code>: </label>
  <input type="text" id="dy" name="dy" value={dy} disabled>
  <br>

  <label for="steps">steps: </label>
  <input type="text" id="steps" name="steps" value={steps} disabled>
  <br>

  <label for="x_inc">x Increment = <code>dx / steps</code>: </label>
  <input type="text" id="x_inc" name="x_inc" value={x_inc} disabled><br>
  <label for="y_inc">y Increment = <code>dy / steps</code>: </label>
  <input type="text" id="y_inc" name="y_inc" value={y_inc} disabled>

  <details>
    <summary>
      Table
    </summary>
    <table>
      <thead>
        <tr>
          <th>Iteration</th>
          <th>x</th>
          <th>y</th>
          <th>x (plot)</th>
          <th>y (plot)</th>
        </tr>
      </thead>

      <tbody>
        {#each x_axis as _, i}
          <tr>
            <td>{i + 1}</td>
            <td>{x_axis[i]}</td>
            <td>{y_axis[i]}</td>
            <td>{x_axis_floored[i]}</td>
            <td>{y_axis_floored[i]}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </details>

  <h3>Graph</h3>
  {#if plotlyReady === false}

    <p class="note">
      Cannot plot graph: Plotly is not ready.
    </p>
  {/if}
{/if}
<div id="algoChart"></div>

<style>
  hr { color: var(--bg-hl-alt-intense); }
  button {
    margin: .8em;
    padding: 0.4em 1.2em;
    width: 100%;
  }
  details {
    border: 1px solid var(--bg-alt);
    border-radius: 4px;
    padding: 0.8em;
    margin: 1em;
  }
</style>
