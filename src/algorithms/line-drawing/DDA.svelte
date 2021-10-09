<script>
  let x0 = 0, y0 = 0, x1 = 4, y1 = 4;

  $: dx = x1 - x0;
  $: dy = y1 - y0;

  $: steps = Math.max(...[dx, dy].map(x => Math.abs(x)));
  $: x_inc = dx / steps;
  $: y_inc = dy / steps;
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
</form>
<hr>

<label for="dx">dx: </label>
<input type="text" id="dx" name="dx" value={dx} disabled><br>
<label for="dy">dy: </label>
<input type="text" id="dy" name="dy" value={dy} disabled>
<hr>

<label for="steps">steps: </label>
<input type="text" id="steps" name="steps" value={steps} disabled>
<hr>

<label for="x_inc">x Increment: </label>
<input type="text" id="x_inc" name="x_inc" value={x_inc} disabled><br>
<label for="y_inc">y Increment: </label>
<input type="text" id="y_inc" name="y_inc" value={y_inc} disabled>
<hr>

<style>
  hr { color: var(--bg-hl-alt-intense); }
</style>
