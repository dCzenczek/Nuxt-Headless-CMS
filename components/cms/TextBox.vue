<template>
  <div class="text-box text-justify">
    {{ description }}
  </div>
</template>

<script>
export default {
  name: 'TextBox',

  props: {
    description: {
      type: String,
      default: ''
    }
  }
}
</script>

<style lang="scss" scoped>
.text-box {
  width: 100%;
}
</style>
