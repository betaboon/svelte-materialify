<script>
  import { getContext } from 'svelte';
  import layoutContextKey from '../Layout/context';
  import { fade } from 'svelte/transition';
  import Style from '../../internal/Style';

  let klass = '';
  export { klass as class };
  export let active = true;
  export let dismissable = true;
  export let mini = false;
  export let clipped = false;

  export let widthRegular = 256;
  export let widthMini = 56;

  export let fixed = false;
  export let absolute = false;
  export let right = false;
  export let borderless = false;
  export let clippedHeight = '56px';
  export let transition = fade;
  export let transitionOpts = {};
  export let index = 4;
  export let style = null;
  export let layout = false;

  const styleVars = {
    'navigation-drawer-width-regular': `${widthRegular}px`,
    'navigation-drawer-width-mini': `${widthMini}px`,
    'nav-clipped-height': `${clippedHeight}px`,
  };

  let layoutContext;
  let appBarHeight;
  if (layout) {
    layoutContext = getContext(layoutContextKey);
    appBarHeight = layoutContext.appBar.height;
  }

  $: if (layoutContext) {
    if (active && mini) {
      layoutContext.navigationDrawer.width.set(widthMini);
    } else if (active) {
      layoutContext.navigationDrawer.width.set(widthRegular);
    } else {
      layoutContext.navigationDrawer.width.set(0);
    }
    layoutContext.navigationDrawer.dismissable.set(dismissable);
    layoutContext.navigationDrawer.clipped.set(clipped);
    styleVars['app-bar-height'] = $appBarHeight;
  }
</script>

<style lang="scss" src="./NavigationDrawer.scss" global>
</style>

<nav
  class="s-navigation-drawer {klass}"
  transition:transition={transitionOpts}
  on:introstart
  on:outrostart
  on:introend
  on:outroend
  class:active
  class:fixed
  class:absolute
  class:right
  class:mini
  class:clipped
  on:hover
  use:Style={styleVars}
  style="z-index:{index};{style}">
  {#if !clipped && dismissable}
    <div class="s-navigation-drawer__app_bar">
      <slot name="app-bar" />
    </div>
  {/if}
  <slot name="prepend" />
  <div class="s-navigation-drawer__content">
    <slot />
  </div>
  <slot name="append" />
  {#if !borderless}
    <div class="s-navigation-drawer__border" />
  {/if}
</nav>
