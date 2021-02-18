<script>
  let klass = '';
  export { klass as class };
  export let style = null;
</script>

<style lang="scss" src="./CardSubtitle.scss">
</style>

<div class="s-card-subtitle {klass}" {style}>
  <slot />
</div>
