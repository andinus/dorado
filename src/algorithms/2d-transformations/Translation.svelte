<script>
  import { onMount } from "svelte";
  import Plotly from "plotly.js-basic-dist-min";

  let points = [];
  let newPoint = {x: 0, y: 0};

  let translate = {x: 0, y: 0};

  function addPoint() {
    // Append to the list.
    points = [
      ...points, {
        x: newPoint.x,
        y: newPoint.y,
        id: Date.now(),
      }
    ];
    newPoint = {x: 0, y: 0}
  }

  function deletePoint(id) {
    points = points.filter(item => item.id !== Number(id));
  }
let translatedPointsArray = {x: [], y: []};

  function translatePoints() {
    // Reset values.
    translatedPointsArray = {x: [], y: []};

    let pointsArray = {x: [], y: []};

    // Store the points in a matrix like array.
    for (const point of points) {
      pointsArray.x = [...pointsArray.x, point.x];
      pointsArray.y = [...pointsArray.y, point.y];
    }

    // Translate the points.
    for (const x of pointsArray.x)
      translatedPointsArray.x.push(x + translate.x);

    for (const y of pointsArray.y)
      translatedPointsArray.y.push(y + translate.y);

    // Plot graphs.
    Plotly.newPlot('algoChart', [{
      x: pointsArray.x,
      y: pointsArray.y,
      mode: 'markers',
      marker: { size: 6 },
      name: 'Original Points'
    }, {
      x: translatedPointsArray.x,
      y: translatedPointsArray.y,
      mode: 'markers',
      marker: { size: 12 },
      name: 'Translated Points'
    }]);
  }

</script>

<h2>Translation</h2>

<form>
  <input type="number" aria-label="x co-ordinate" placeholder="0"
         bind:value={newPoint.x}>
  <input type="number" aria-label="y co-ordinate" placeholder="0"
         bind:value={newPoint.y}>
  <button type="button" on:click={addPoint}>Add Point</button>
</form>

<h4>Translation co-ordinates</h4>
<form>
  <input type="number" aria-label="x co-ordinate" placeholder="0"
         bind:value={translate.x}>
  <input type="number" aria-label="y co-ordinate" placeholder="0"
         bind:value={translate.y}>
</form>

{#if points.length > 0}
  <button id="translatePoints" type="button"
          on:click={translatePoints}>Translate Points</button>
{/if}
<hr>
{#if points.length > 0}
  <h4>Original Points</h4>
  <table>
    <thead>
      <tr>
        <th style="width: 2em;">Delete</th>
        <th>x</th>
        <th>y</th>
      </tr>
    </thead>
    <tbody>
      {#each points as point (point.id)}
        <tr>
          <td>
            <button type="button"
                    on:click={deletePoint(point.id)}>&nbsp;X&nbsp;</button>
          </td>
          <td>{point.x}</td>
          <td>{point.y}</td>
        </tr>
      {/each}
    </tbody>
  </table>
  <h4>Translation Points</h4>
  <table>
    <thead>
      <tr>
       
        <th>x</th>
        <th>y</th>
      </tr>
    </thead>
    <tbody>
      {#each translatedPointsArray.x as _, i}
        <tr>
          <td>{translatedPointsArray.x[i]}</td>
          <td>{translatedPointsArray.y[i]}</td>
        </tr>
      {/each}
    </tbody>
  </table>
{/if}

<div id="algoChart"></div>

<style>
  #translatePoints {
    margin: .8em;
    padding: 0.4em 1.2em;
    width: 100%;
  }
  hr { color: var(--bg-hl-alt-intense); }
</style>
