<template>
  <div>
    <h3 class="category-title">{{title}}</h3>
    <div class="category">
        <figure v-for="artist in artists" :key="artist.id"  :class="isCurrentArtist(artist.id)">
          <img class="artist-image" @click="handleClick" :data-id="artist.id" :src="createUrl(artist.image)" alt="">
          <figcaption class="artist-info">
            <p class="artist-name">{{ artist.name }}</p>
            <p class="artist-category">{{ artist.category }}</p>
          </figcaption>
        </figure>
      </div>
  </div>
</template>
<script>
export default {
  name: 'Artist',
  props: {
    title: String,
    artists: Array
  },
  methods: {
    isCurrentArtist (id) {
      return id === Number(this.$route.params.id) ? 'tile tile--slide-in' : 'tile'
    },
    createUrl (image) {
      return require(`@/assets/${image}`)
    },
    handleClick (e) {
      const currentEl = e.target
      const artistId = currentEl.dataset.id
      const { x, y } = currentEl.getBoundingClientRect()
      const position = { left: x, top: y }
      this.$root.$emit('gotoTracks', artistId, position)

      currentEl.style.opacity = 0
    }
  }
}
</script>
<style lang="scss" scoped>
$tile-colors: #48494b, #727ba5, #849cb0, #9d9ea6;
$easing: cubic-bezier(0.19, 1, 0.22, 1);

:root {
  --translate-tile: none;
}

.category-title {
  margin-bottom: 30px;
}

.category {
  // outline: 1px dashed var(--red);
  display: grid;
  grid-template-columns: repeat(auto-fill, 190px);
  grid-column-gap: 40px;
}

.tile {
  // outline: 1px dashed red;
  position: relative;
  margin-bottom: 30px;
  border-radius: 10px;
  text-align: center;

  @for $i from 1 to length($tile-colors) + 1 {
    &:nth-child(#{$i}) {
      background-color: nth($tile-colors, $i);

      &::before {
        background-color: nth($tile-colors, $i);
        border-color: nth($tile-colors, $i);
      }
    }
  }

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    width: 100%;
    height: 100%;
    border: 4px solid;
    border-radius: 10px;
    transition: transform .4s var(--easing-smooth);
  }

  &:hover::before {
    transform: scale(1.05, 1.04);
  }

  &--slide-in {
    animation: tile-move .4s $easing;
  }
}

@keyframes tile-move {
  from {
    transform-origin: left top;
    transform: var(--translate-tile)
  }
  to {
    transform-origin: left top;
  }
}

.artist {
  &-image {
    width: 190px;
    height: 190px;
    border-radius: 10px 10px 0 0;
    object-fit: cover;
  }

  &-info {
    padding: 10px 0;
  }

  &-name {
    line-height: 1;
  }

  &-category {
    font-size: 14px;
    line-height: 1.2;
  }
}
</style>
