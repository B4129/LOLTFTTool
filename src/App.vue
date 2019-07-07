<template >
  <v-app class="theme--dark">
    <v-toolbar app class="theme--dark">
      <v-toolbar-title class="headline text-uppercase ">
        <span class="font-weight-light" >LOLTFTItemBuilder</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <ItemBuilder/>
    </v-content>
  </v-app>
</template>

<script>
import ItemBuilder from './components/Itembuilder'

export default {
  name: 'App',
  components: {
    ItemBuilder: ItemBuilder
  },
  data () {
    return {
      //
    }
  }
}
</script>
