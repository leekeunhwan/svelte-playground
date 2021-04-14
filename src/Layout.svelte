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
    background-color: #eaecef;
  }

  .layout__inner {
    width: 550px;
    height: 100vh;
    background-color: #ffffff;
  }
</style>
