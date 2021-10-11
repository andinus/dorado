<script>
  import { createEventDispatcher } from 'svelte';

  import Home from './Home.svelte';
  import DDA from './algorithms/line-drawing/DDA.svelte';
  import Bresenham from './algorithms/line-drawing/Bresenham.svelte';
  import Scaling from './algorithms/2d-transformations/Scaling.svelte';

  const dispatch = createEventDispatcher();
  const changePage = (p) => dispatch('message', { page: p });
</script>

<nav class="ddmenu">
  <button on:click={() => changePage(Home)}>Home</button>

  <div class="ddgroup">
    Line Drawing Algorithms
    <div class="ddsub">
      <button on:click={() => changePage(DDA)}>
        Digital differential analyzer
      </button>
      <button on:click={() => changePage(Bresenham)}>
        Bresenham
      </button>
    </div>
  </div>

  <div class="ddgroup">
    2 Dimensional Transformations
    <div class="ddsub">
      <button on:click={() => changePage(Scaling)}>
        Scaling
      </button>
      <button on:click={() => alert("Not Implemented.")}>
        Translation
      </button>
    </div>
  </div>
</nav>

<style>
  .ddmenu {
    display: flex;
    padding: 0.2em;
    border-bottom: 1px solid var(--bg-alt);
  }

  .ddmenu button, .ddmenu .ddgroup {
    text-align: left;
    width: 100%;
    padding: 8px;
    color: var(--fg-main);
    text-decoration: none;
    background: var(--bg-alt);
    border: 2px solid var(--bg-main);
  }
  .ddmenu button:hover, .ddmenu .ddgroup:hover {
    color: var(--fg-special-cold);
    background: var(--bg-dim);
    cursor: pointer;
  }
  .ddmenu .ddgroup { position: relative; }
  .ddsub {
    width: 100%;
    position: absolute;
    top: 100%; left: 0;
    display: none;
  }
  .ddsub button {
    display: block;
    box-sizing: border-box;
  }
  .ddgroup:hover .ddsub { display: block; }
  .ddgroup::after {
    content: "\27A4";
    position: absolute;
    top: 8px; right: 8px;
    transition: transform 0.2s;
  }
  .ddgroup:hover::after {
    transform: rotate(90deg);
  }
  @media screen and (max-width: 640px) {
    .ddmenu { flex-wrap: wrap; }
    .ddsub {
      position: static;
      margin-top: 10px;
    }
    .ddmenu button, .ddmenu .ddgroup {
      padding: 12px;
    }
  }
</style>
