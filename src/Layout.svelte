<div class="layout__outer">
  <div class="layout__inner">
    <slot />
  </div>
</div>

<style>
  .layout__outer {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .layout__inner {
    width: 650px;
    height: 100vh;
  }
</style>
