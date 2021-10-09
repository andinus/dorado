<script>
  import Navbar from './Navbar.svelte'

  import Home from './Home.svelte'
  import DDA from './algorithms/line-drawing/DDA.svelte'

  let page = Home
</script>

<h1 class="title">CG Algorithms</h1>
<Navbar />
<main>
  <svelte:component this={page} />
</main>

<style>
</style>
