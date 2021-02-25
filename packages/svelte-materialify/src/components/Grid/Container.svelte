<script>
  let klass = '';
  export let fluid = false;
  export let style = null;
  export { klass as class };
</script>

<style lang="scss">
  @use '../../styles/config/config-loader' as config;

  $padding: config.get('components', 'Container', 'padding');

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
