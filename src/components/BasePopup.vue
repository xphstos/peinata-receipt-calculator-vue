<template>
  <div class="popup" :class="{ 'popup--open': isOpen }">
    <div class="popup__body">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup",
  props: {
    isOpen: {
      type: Boolean,
      default: false
    }
  }
};
</script>
