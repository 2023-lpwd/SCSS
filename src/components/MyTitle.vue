<script>
export default {
  data() {
    return {
      className: `title -${this.size}`,
    };
  },
  props: {
    size: String,
    type: String,
    label: String,
  },
};
</script>

<template>
  <h2 v-if="type === 'h2'" :class="className">{{ label }}</h2>
  <h3 v-else-if="type === 'h3'" :class="className">{{ label }}</h3>
  <h1 v-else :class="className">{{ label }}</h1>
</template>

<style lang="scss" scoped>
@import "./../scss/foundations/variables";
.title {
  font-family: $titleFontFamily;
  font-size: 2rem;
  &.-tiny {
    font-size: 1rem;
  }
  &.-big {
    font-size: 3rem;
  }
}
</style>
