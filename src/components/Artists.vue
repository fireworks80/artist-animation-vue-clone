<template>
  <section>
    <h2 class="title">Artists</h2>
    <Artist title="Trending Now" :artists="artists" />
    <Artist title="Popular" :artists="popular" />
  </section>
</template>
<script>
import artistsData from '@/data/artists'
import Artist from './Artist'

export default {
  name: 'Artists',
  components: { Artist },
  data () {
    return {
      artists: artistsData.filter(artist => [1, 2, 3, 4].includes(artist.id)),
      popular: artistsData.filter(artist => [5, 6].includes(artist.id))
    }
  },
  beforeMount () {
    if (!this.$route.params.id) return

    const rootEl = document.documentElement
    const { left, top } = this.$route.params.position
    const offsetLeft = 100
    const offsetTop = 30
    const x = -(left - offsetLeft)
    const y = -(top - offsetTop)

    rootEl.style.setProperty('--translate-tile', `translate(${x}px, ${y}px) scale(${240 / 190})`)
  }
}
</script>
<style lang="scss" scoped>

section {
  padding: {
    top: 50px;
    left: 100px;
  }
}

.title {
  color: var(--red);
}

</style>
