<script>
  import { writable } from 'svelte/store';
  import { readonly } from 'svelte-readonly';
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
  export let clippedHeight = 56;
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
  if (layout) {
    layoutContext = getContext(layoutContextKey);
  }

  const widthStore = writable(null);
  const clippedStore = writable(null);
  const dismissableStore = writable(null);
  if (layoutContext) {
    $layoutContext.navigationDrawer = {
      width: readonly(widthStore),
      clipped: readonly(clippedStore),
      dismissable: readonly(dismissableStore),
    };
  }

  $: if (active && mini) {
    widthStore.set(widthMini);
  } else if (active) {
    widthStore.set(widthRegular);
  } else {
    widthStore.set(0);
  }
  $: clippedStore.set(clipped);
  $: dismissableStore.set(dismissable);

  let appBarHeight = readonly(writable(0));
  $: try {
    appBarHeight = $layoutContext.appBar.height;
  } catch {
    /**/
  }

  $: styleVars['app-bar-height'] = $appBarHeight;
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
