<script>
  let klass = '';
  export { klass as class };
  export let style = null;
</script>

<style lang="scss" src="./CardActions.scss">
</style>

<div class="s-card-actions {klass}" {style}>
  <slot />
</div>
