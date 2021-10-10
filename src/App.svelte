<script>
  import Navbar from './Navbar.svelte';
  import Home from './Home.svelte';

  let page = Home;
  const changePage = event => page = event.detail.page;
</script>

<div id="org-div-home-and-up">
  <a href="https://andinus.unfla.me/projects/dorado">Project Homepage</a>
</div>
<h1 class="title">
  Dorado
  <br>
  <span class="subtitle">
    Collection of Algorithms used in Computer Graphics
  </span>
</h1>
<Navbar on:message={changePage} />
<main>
  <svelte:component this={page} />
</main>
<div id="postamble" class="status">
  <p class="postamble">Made with <span class="heart">♥</span> by
    <a href="https://andinus.unfla.me">Andinus</a>
    <span style="float:right"><a href="https://git.unfla.me/dorado">Source
    Code</a> / <a href="https://github.com/andinus/dorado">GitHub (mirror)</a></span></p>
</div>
