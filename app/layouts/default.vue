<template>
  <div class="app-layout">
    <SideMenu />

    <div class="main-content">

      <slot />
    </div>
  </div>
</template>

<style scoped>

  body{
    background-color: rgb(15, 15, 15);
    color: white;
  }
  .app-layout {
    display: flex;
    min-height: 100vh;
  }
  .main-content {
    flex: 1;
  }
</style>