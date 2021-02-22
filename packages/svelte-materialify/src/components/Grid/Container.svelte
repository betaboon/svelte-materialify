<script>
  let klass = '';
  export let fluid = false;
  export let style = null;
  export { klass as class };
</script>

<style lang="scss">
  @use '../../styles/config';
  $padding: config.get('components', 'container', 'padding');

  .s-container {
    width: 100%;
    padding: $padding;
    margin-right: auto;
    margin-left: auto;

    &.fluid {
      max-width: 100%;
    }
  }
</style>

<div class="s-container {klass}" class:fluid {style}>
  <slot />
</div>
