<script>
  import { setContext } from 'svelte';
  import { writable } from 'svelte/store';
  import Style from '../../internal/Style';
  import layoutContextKey from './context';

  import MaterialApp from '../MaterialApp';
  import Card from '../Card';
  import List, { ListItem, ListItemGroup } from '../List';
  import Subheader from '../Subheader';

  export let theme = 'light';
  export let debug = false;

  const layoutContext = {
    appBar: {
      height: writable(null),
    },
    navigationDrawer: {
      width: writable(null),
      dismissable: writable(null),
      clipped: writable(null),
    },
  };

  setContext(layoutContextKey, layoutContext);

  const { height: appBarHeight } = layoutContext.appBar;
  const {
    width: navigationDrawerWidth,
    dismissable: navigationDrawerDismissable,
    clipped: navigationDrawerClipped,
  } = layoutContext.navigationDrawer;

  let styleVars = {};
  $: {
    styleVars = {
      'navigation-drawer-width': $navigationDrawerWidth,
      'app-bar-height': $appBarHeight,
    };
  }
</script>

<style lang="scss">
  @import './variables';

  :global(.s-app) {
    height: 100%;
  }
  :global(.s-app-bar) {
    flex: 0 1 auto;
  }

  .s-layout {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .s-layout-app-bar {
    transition-duration: 0.2s;
    /* transition-timing-function: map-get($transitions, 'fast-out-slow-in'); */
    /* will-change: transform; */
    transition-property: margin-left;
  }

  .full-height-navigation-drawer .s-layout-app-bar {
    margin-left: var(--s-navigation-drawer-width);
    height: 0;
  }

  .s-layout-body {
    display: flex;
    flex: 1;
    overflow: hidden;
  }

  .s-layout-body > * {
    display: contents;
  }

  .s-layout-main > :global(*) {
    flex: 1;
    overflow: auto;
  }

  .full-height-navigation-drawer .s-layout-main > :global(*) {
    margin-top: var(--s-app-bar-height);
  }

  .debug {
    position: absolute;
    bottom: 0;
    right: 0;
    z-index: 100;
  }
</style>

<MaterialApp {theme}>
  <div
    class="s-layout"
    use:Style={styleVars}
    class:full-height-navigation-drawer={!$navigationDrawerClipped}>
    <div class="s-layout-app-bar">
      <slot name="app-bar" />
    </div>
    <div class="s-layout-body">
      <div class="s-layout-navigation-drawer">
        <slot name="navigation-drawer" />
      </div>
      <div class="s-layout-main">
        <slot />
      </div>
    </div>
  </div>
  {#if debug}
    <div class="debug">
      <Card>
        <List dense disabled class="elevation-2">
          <Subheader>AppBar</Subheader>
          <ListItemGroup class="blue-text" value={[1]}>
            <ListItem>Height: {$appBarHeight}</ListItem>
          </ListItemGroup>
          <Subheader>NavigationDrawer</Subheader>
          <ListItemGroup class="blue-text" value={[1]}>
            <ListItem>Width: {$navigationDrawerWidth}</ListItem>
            <ListItem>Dismissable: {$navigationDrawerDismissable}</ListItem>
            <ListItem>Clipped: {$navigationDrawerClipped}</ListItem>
          </ListItemGroup>
        </List>
      </Card>
    </div>
  {/if}
</MaterialApp>
