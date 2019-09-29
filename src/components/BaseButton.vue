<template>
  <button
    class="btn"
    :class="{ 'btn--primary': primary, 'btn--outlined': outlined }"
    @click="clicked"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "BaseButton",
  props: {
    primary: {
      type: Boolean,
      default: true
    },
    outlined: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    clicked() {
      this.$emit("boom");
    }
  }
};
</script>
