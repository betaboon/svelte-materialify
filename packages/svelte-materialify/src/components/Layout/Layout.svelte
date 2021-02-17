<script>
  import { writable } from 'svelte/store';
  import { setContext } from 'svelte';
  import layoutContextKey from './context';

  import Style from '../../internal/Style';

  import MaterialApp from '../MaterialApp';
  import Card from '../Card';
  import List, { ListItem, ListItemGroup } from '../List';
  import Subheader from '../Subheader';

  export let theme = 'light';
  export let debug = false;

  const layoutContext = writable({});
  setContext(layoutContextKey, layoutContext);

  let appBarHeight;
  $: try {
    appBarHeight = $layoutContext.appBar.height;
  } catch {
    /**/
  }

  let navigationDrawerWidth;
  let navigationDrawerClipped;
  let navigationDrawerDismissable;
  $: try {
    navigationDrawerWidth = $layoutContext.navigationDrawer.width;
    navigationDrawerClipped = $layoutContext.navigationDrawer.clipped;
    navigationDrawerDismissable = $layoutContext.navigationDrawer.dismissable;
  } catch {
    /**/
  }

  const styleVars = {};
  $: styleVars['app-bar-height'] = $appBarHeight;
  $: styleVars['navigation-drawer-width'] = $navigationDrawerWidth;
</script>

<style lang="scss" src="./Layout.scss">
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
