<template>
  <v-app>
    <list></list>
  </v-app>
</template>

<script>
import list from './components/List'

export default {
  name: 'App',
  components: {
    list
  },
  data () {
    return {
      nombre: "alejandro"
    }
  }
}
</script>
