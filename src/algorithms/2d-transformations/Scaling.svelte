<script>
  import { onMount } from "svelte";
  import Plotly from "plotly.js-basic-dist-min";

  onMount(() => {
    Plotly.newPlot('algoChart', [{
      x: [1, 2],
      y: [2, 3],
      type: 'lines',
      name: 'Scaling'
    }]);
  });
</script>

<h2>Scaling</h2>
<div id="algoChart"></div>
<style>
</style>
