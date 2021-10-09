<script>
  import Navbar from './Navbar.svelte'
  import Home from './Home.svelte'

  let page = Home
  const changePage = event => page = event.detail.page;
</script>

<h1 class="title">CG Algorithms</h1>
<Navbar on:message={changePage} />
<main>
  <svelte:component this={page} />
</main>
