<script>
  import { onMount } from "svelte";
  import Plotly from "plotly.js-basic-dist-min";

  let invalidInput = false;

  let x0 = 0, y0 = 0, x1 = 4, y1 = 4;
  let dx, dy, step;

  let x_axis = [], y_axis = [], p_vals = [];

  function solve() {
    dx = Math.abs(x1 - x0);
    dy = Math.abs(y1 - y0);

    // Early return if slope is not between 0 - 1.
    if (dy > dx) {
      invalidInput = true;
      return;
    } else {
      invalidInput = false;
    }

    let p = (2 * dy) - dx;
    p_vals.push(p);

    step = 2 * (dy - dx);

    // Build the x axis and y axis.
    x_axis = [x0,];
    y_axis = [y0,];

    for (let idx = 0; idx < dx; idx++) {
      // x_axis.at(-1) doesn't work for some reason.
      x_axis.push(x_axis[x_axis.length - 1] + 1);
      if (p < 0) {
        y_axis.push(y_axis[y_axis.length - 1]);
        p += 2 * dy;
      } else {
        y_axis.push(y_axis[y_axis.length - 1] + 1);
        p += step;
      }
      p_vals.push(p);
    }

    // Plot the chart.
    Plotly.newPlot('algoChart', [{
      x: x_axis,
      y: y_axis,
      type: 'lines',
      name: 'Bresenham'
    }]);
  }

  onMount(() => {
    solve();
  });
</script>

<h2>Bresenham</h2>
<form class="points">
  {#if invalidInput === true}
    <p class="note">
      Invalid Input: Slope must be between 0 to 1.
    </p>
  {/if}
  <label for="x0">x0: </label>
  <input type="number" id="x0" name="x0" on:change={solve} bind:value={x0}>&nbsp;
  <label for="y0">y0: </label>
  <input type="number" id="y0" name="y0" on:change={solve} bind:value={y0}>
  <br>
  <label for="x1">x1: </label>
  <input type="number" id="x1" name="x1" on:change={solve} bind:value={x1}>&nbsp;
  <label for="y1">y1: </label>
  <input type="number" id="y1" name="y1" on:change={solve} bind:value={y1}>
  <br>
  <button type="button" on:click={solve}>Solve</button>
</form>

<hr>
<h3>Solution</h3>

{#if invalidInput === true}
  <p class="note">
    Invalid Input: Slope must be between 0 to 1.
  </p>
{/if}

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>dx = <code>|x1 - x0|</code></td>
      <td>{dx}</td>
    </tr>
    <tr>
      <td>dy = <code>|y1 - y0|</code></td>
      <td>{dy}</td>
    </tr>
    <tr>
      <td>step = <code>|2 * (dy - dx)|</code></td>
      <td>{step}</td>
    </tr>
    <tr>
      <td><code>2 * dy</code></td>
      <td>{2 * dy}</td>
    </tr>
  </tbody>
</table>

<details>
  <summary>
    Table
  </summary>
  <table>
    <thead>
      <tr>
        <th>Iteration (k)</th>
        <th>P<sub>k</sub></th>
        <th>x<sub>k + 1</sub></th>
        <th>y<sub>k + 1</sub></th>
      </tr>
    </thead>

    <tbody>
      {#each x_axis as _, i}
        <tr>
          <td>{i}</td>
          <td>{p_vals[i]}</td>
          <td>{x_axis[i]}</td>
          <td>{y_axis[i]}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</details>

<h3>Graph</h3>
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
