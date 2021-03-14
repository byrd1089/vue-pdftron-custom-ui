<template>
  <div class="sidenav" :class="{right: right, open: open}">
    <slot />
  </div>
</template>

<script>
export default {
  name: 'Drawer.vue',
  props: {
    open: {
      type: Boolean,
      default: false
    },
    right: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped>
.sidenav {
  height: calc(100% - 100px);
  width: 0; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  top: 100px;
  left: 0;
  overflow-x: hidden; /* Disable horizontal scroll */
  transition: 0.5s; /* 0.5 second transition effect to slide in the sidenav */
}
.sidenav.open {
  width: 25%;
}
.sidenav.right {
  left:auto;
  right: 0;
}
</style>