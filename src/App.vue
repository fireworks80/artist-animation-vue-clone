<template>
  <div id="app">
    <router-view :gotoTracks="gotoTracks" :gotoArtists="gotoArtists"/>
  </div>
</template>
<script>
export default {
  name: 'app',
  data () {
    return {
      artistId: null,
      position: null
    }
  },
  created () {
    this.$root.$on('gotoTracks', (id, position) =>
      this.gotoTracks(id, position)
    )
    this.$root.$on('gotoArtists', () => this.gotoArtists())
  },
  methods: {
    gotoTracks (id, position) {
      this.artistId = id
      this.position = position

      this.$router.push({
        name: 'tracks',
        params: {
          id: id,
          position: position
        }
      })
    },
    gotoArtists () {
      this.$router.push({
        name: 'artists',
        params: {
          id: this.artistId,
          position: this.position
        }
      })
    }
  }
}
</script>

<style lang="scss">
</style>
