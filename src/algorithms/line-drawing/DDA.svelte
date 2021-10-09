<script>
  let solved = false;

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

    solved = true;
  }

  // Solve with prefilled values.
  dda();
</script>

<h2>Digital differential analyzer</h2>
<form class="points">
  <label for="x0">x0: </label>
  <input type="number" id="x0" name="x0" bind:value={x0}>&nbsp;
  <label for="y0">y0: </label>
  <input type="number" id="y0" name="y0" bind:value={y0}>
  <br>
  <label for="x1">x1: </label>
  <input type="number" id="x1" name="x1" bind:value={x1}>&nbsp;
  <label for="y1">y1: </label>
  <input type="number" id="y1" name="y1" bind:value={y1}>
  <br>
  <button type="button" on:click={dda}>Solve</button>
</form>

{#if solved === true && steps > 0}
  <hr>
  <label for="dx">dx = <code>|x1 - x0|</code>: </label>
  <input type="text" id="dx" name="dx" value={dx} disabled><br>
  <label for="dy">dy = <code>|y1 - y0|</code>: </label>
  <input type="text" id="dy" name="dy" value={dy} disabled>
  <hr>

  <label for="steps">steps: </label>
  <input type="text" id="steps" name="steps" value={steps} disabled>
  <hr>

  <label for="x_inc">x Increment = <code>dx / steps</code>: </label>
  <input type="text" id="x_inc" name="x_inc" value={x_inc} disabled><br>
  <label for="y_inc">y Increment = <code>dy / steps</code>: </label>
  <input type="text" id="y_inc" name="y_inc" value={y_inc} disabled>
  <hr>

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
{/if}

<style>
  hr { color: var(--bg-hl-alt-intense); }
</style>
