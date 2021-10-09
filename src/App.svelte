<script>
  import Navbar from './Navbar.svelte';
  import Home from './Home.svelte';

  import DDA from './algorithms/line-drawing/DDA.svelte';
  let page = DDA;
  const changePage = event => page = event.detail.page;
</script>

<h1 class="title">CG Algorithms</h1>
<Navbar on:message={changePage} />
<main>
  <svelte:component this={page} />
</main>
