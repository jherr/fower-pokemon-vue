<template>
  <button
    @click="onClick"
    p-10
    textLG
    w="80%"
    :white="selected"
    :bgBlue600="selected"
    :border-1="!selected"
    :borderBlue600="!selected"
    roundedFull
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: "PillButton",
  props: ["selected"],
  methods: {
    onClick() {
      this.$emit("click")
    }
  }
}
</script>