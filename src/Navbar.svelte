<nav class="ddmenu">
  <a href="#">Home</a>

  <div class="ddgroup">
    Line Drawing Algorithms
    <div class="ddsub">
      <a href="#">DDA</a>
      <a href="#">Bresenham</a>
    </div>
  </div>
</nav>

<style>
  .ddmenu { display: flex; }

  .ddmenu a, .ddmenu .ddgroup {
    width: 100%;
    padding: 15px;
    color: var(--fg-main);
    text-decoration: none;
    background: var(--bg-alt);
    border: 2px solid var(--bg-main);
  }
  .ddmenu a:hover, .ddmenu .ddgroup:hover {
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
  .ddsub a {
    display: block;
    box-sizing: border-box;
  }
  .ddgroup:hover .ddsub { display: block; }
  .ddgroup::after {
    content: "\27A4";
    position: absolute;
    top: 13px; right: 10px;
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
  }
</style>
