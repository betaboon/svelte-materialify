<script>
  import { writable } from 'svelte/store';
  import { readonly } from 'svelte-readonly';
  import { getContext } from 'svelte';
  import layoutContextKey from '../Layout/context';

  import Style from '../../internal/Style';

  let klass = '';
  export { klass as class };
  export let heightRegular = 56;
  export let heightDense = 48;
  export let heightProminent = 128;
  export let tile = false;
  export let flat = false;
  export let dense = false;
  export let prominent = false;
  export let fixed = false;
  export let absolute = false;
  export let collapsed = false;
  export let style = '';
  export let layout = false;

  const styleVars = {
    'app-bar-height-regular': `${heightRegular}px`,
    'app-bar-height-dense': `${heightDense}px`,
    'app-bar-height-prominent': `${heightProminent}px`,
  };

  let layoutContext;
  if (layout) {
    layoutContext = getContext(layoutContextKey);
  }

  const heightStore = writable(null);
  if (layoutContext) {
    $layoutContext.appBar = {
      height: readonly(heightStore),
    };
  }

  $: if (prominent) {
    heightStore.set(heightProminent);
  } else if (dense) {
    heightStore.set(heightDense);
  } else {
    heightStore.set(heightRegular);
  }

  let navigationDrawerDismissable = readonly(writable(true));
  $: try {
    navigationDrawerDismissable = $layoutContext.navigationDrawer.dismissable;
  } catch {
    /**/
  }
</script>

<style lang="scss" src="./AppBar.scss" global>
</style>

<header
  class="s-app-bar {klass}"
  class:tile
  class:flat
  class:dense
  class:prominent
  class:fixed
  class:absolute
  class:collapsed
  use:Style={styleVars}
  {style}>
  <div class="s-app-bar__wrapper">
    {#if $navigationDrawerDismissable}
      <slot name="icon" />
    {/if}
    {#if !collapsed}
      <div class="s-app-bar__title">
        <slot name="title" />
      </div>
    {/if}
    <slot />
  </div>
  <slot name="extension" />
</header>
