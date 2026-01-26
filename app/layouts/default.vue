<template>
  <div class="app-layout">
    <SideMenu />

    <div class="main-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.app-layout {
  display: flex;
  min-height: 100vh;
}
.main-content {
  flex: 1;
}
</style>