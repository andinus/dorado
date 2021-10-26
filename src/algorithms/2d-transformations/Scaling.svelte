<script>
  import { onMount } from "svelte";
  import Plotly from "plotly.js-basic-dist-min";

  let to_add_x = 0, to_add_y = 0;
  let fixed_x = 0, fixed_y = 0;

  let points = [[0, 1], [0, 1]];
  let points_str_x = to_matrix(points[0]);
  let points_str_y = to_matrix(points[1]);
  let points_after_fixed = JSON.parse(JSON.stringify(points));

  let scale_factor = {x: 2, y: 2};

  let scaled_points = [[0, 2], [0, 2]];
  let scaled_points_after_fixed = JSON.parse(JSON.stringify(scaled_points));

  function to_matrix(mat) { return JSON.stringify(mat, null, '\t') }

  function add_points() {
    points[0].push(to_add_x);
    points[1].push(to_add_y);
    to_add_x = 0;
    to_add_y = 0;
    solve();
  }

  function clear_points() {
    points[0] = [];
    points[1] = [];
    solve();
  }

  function solve() {
    const scale_mat = [[scale_factor.x, 0], [0, scale_factor.y]];

    scaled_points = [[], []];
    // Deep copy.
    points_after_fixed = JSON.parse(JSON.stringify(points));
    for (let i = 0; i < points[0].length; i++) {
      scaled_points[0].push(0);
      scaled_points[1].push(0);

      points_after_fixed[0][i] -= fixed_x;
      points_after_fixed[1][i] -= fixed_y;
    }

    for (var i = 0; i < 2; i++)
      for (var j = 0; j < points[0].length; j++)
        for (var k = 0; k < points.length; k++)
          scaled_points[i][j] += scale_mat[i][k] * points_after_fixed[k][j];

    scaled_points_after_fixed = JSON.parse(JSON.stringify(scaled_points));
    for (let i = 0; i < scaled_points[0].length; i++) {
      scaled_points_after_fixed[0][i] += fixed_x;
      scaled_points_after_fixed[1][i] += fixed_y;
    }

    points_str_x = to_matrix(points[0]);
    points_str_y = to_matrix(points[1]);

    Plotly.newPlot('algoChart', [{
      x: points[0],
      y: points[1],
      line: { width: 2 },
      type: 'lines',
      name: 'Original'
    }, {
      x: scaled_points_after_fixed[0],
      y: scaled_points_after_fixed[1],
      line: { width: 1 },
      type: 'lines',
      name: 'Scaled'
    }], { title: "Scaling Transformation" });
  }

  onMount(() => { solve(); });
</script>

<h2>Scaling Transformation</h2>
<p class="note">
  The graph might not print the expected figure. That is because the
  order of points matter but the final answer should be correct.
</p>
<p class="note">
  To solve: clear points, add required points, update fixed point,
  update scale factor and click solve.
</p>

<form class="points">
  <section>
    <p>
      Points (P):<br>
      <div class="mat">
        x: {points_str_x}
        <br>
        y: {points_str_y}
      </div>
    <p>
  </section>
  <section>
    <label for="to_add_x">x: </label>
    <input type="number" id="to_add_x" name="to_add_x" bind:value={to_add_x}>&nbsp;
    <label for="to_add_y">y: </label>
    <input type="number" id="to_add_y" name="to_add_y" bind:value={to_add_y}>
    <br>
    <button type="button" on:click={add_points}>Add Points</button>
    <button type="button" on:click={clear_points}>Clear Points</button>
  </section>
  <section>
    <h4>Fixed Points</h4>
    <label for="fixed_x">x: </label>
    <input type="number" id="fixed_x" name="fixed_x" bind:value={fixed_x}>&nbsp;
    <label for="fixed_y">y: </label>
    <input type="number" id="fixed_y" name="fixed_y" bind:value={fixed_y}>
    <br>
    <button type="button" on:click={solve}>Update Fixed Point</button>
  </section>
  <section>
    <h4>Scale Factor</h4>
    <label for="scale_x">x: </label>
    <input type="number" id="fixed_x" on:change={solve}
           name="scale_x" bind:value={scale_factor.y}>&nbsp;
    <label for="scale_y">y: </label>
    <input type="number" id="scale_y" on:change={solve}
           name="scale_y" bind:value={scale_factor.x}>
    <br>
    <button type="button" on:click={solve}>Update Scale Factor</button>
  </section>
  <button type="button" on:click={solve}>Solve</button>
</form>

<hr>
<h3>Solution</h3>

<p>P<sup>'</sup> = P<sub>f</sub> + S * (P - P<sub>f</sub>)<p>
<p>
P - P<sub>f</sub>:<br>
<div class="mat">
  x: {to_matrix(points_after_fixed[0])}
  <br>
  y: {to_matrix(points_after_fixed[1])}
</div>
<p>

<p>
S * (P - P<sub>f</sub>):<br>
<div class="mat">
  x: {to_matrix(scaled_points[0])}
  <br>
  y: {to_matrix(scaled_points[1])}
</div>
<p>

<p>P<sup>'</sup> = P<sub>f</sub> + S * (P - P<sub>f</sub>)<p>
<p>P<sup>'</sup>:
  <div class="mat">
    x: {to_matrix(scaled_points_after_fixed[0])}
    <br>
    Y: {to_matrix(scaled_points_after_fixed[1])}
  </div>
<p>

<div id="algoChart"></div>
<style>
  hr { color: var(--bg-hl-alt-intense); }
  button {
    margin: .8em;
    padding: 0.4em 1.2em;
    width: 100%;
  }
  section {
    border: 1px dashed var(--fg-alt);
    padding: 0.4em 0.8em;
  }
  .mat {
    padding-left: 2ch;
  }
</style>
