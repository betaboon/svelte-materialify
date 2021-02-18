<script>
  let klass = '';
  export let fluid = false;
  export let style = null;
  export { klass as class };
</script>

<style lang="scss">
  @use "../../styles/tools/breakpoints";
  @import '../../styles/variables';

  .s-container {
    width: 100%;
    padding: $container-padding-x;
    margin-right: auto;
    margin-left: auto;

    &.fluid {
      max-width: 100%;
    }

    @include breakpoints.from('md') {
      max-width: map-get($container-max-widths, 'md');
    }

    @include breakpoints.from('lg') {
      max-width: map-get($container-max-widths, 'lg');
    }

    @include breakpoints.from('xl') {
      max-width: map-get($container-max-widths, 'xl');
    }
  }
</style>

<div class="s-container {klass}" class:fluid {style}>
  <slot />
</div>
