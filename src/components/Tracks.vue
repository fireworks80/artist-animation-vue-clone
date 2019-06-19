<template>
  <div>
    <header class="header">
      <button class="btn-forward" @click="goBack">
        <svg width="14" height="14" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path fill="#FFF" d="M24 10.5H5.7l8.4-8.4L12 0 0 12l12 12 2.1-2.1-8.4-8.4H24z"></path>
        </svg>
      </button>
      <figure class="image-wrapper">
        <img class="image" :src="createUrl(artist.image)" alt>
        <figcaption>
          <p class="artist-name">{{artist.name}}</p>
          <p class="artist-category">{{artist.category}}</p>
        </figcaption>
      </figure>
      <div class="header__btns">
        <button class="shuffle">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" width="16" height="16">
            <path
              fill="#fff"
              d="M512 387.67L405.406 281.024v84.694h-10.435c-68.152 0-102.461-56.146-142.191-121.157-40.72-66.633-86.875-142.158-179.637-142.158H0v43.886h73.143c68.152 0 102.461 56.146 142.191 121.157 40.72 66.633 86.875 142.158 179.637 142.158h10.435v84.686L512 387.67z"
            ></path>
            <path
              fill="#fff"
              d="M512 124.355L405.406 17.71v84.695h-10.435c-66.714 0-109.319 39.063-143.135 85.788 9.055 13.584 17.433 27.249 25.391 40.268 31.024-46.945 63.77-82.172 117.744-82.172h10.435v84.686L512 124.355zM190.887 283.548c-31.024 46.943-63.77 82.17-117.744 82.17H0v43.886h73.143c66.714 0 109.319-39.063 143.135-85.788-9.056-13.584-17.433-27.249-25.391-40.268z"
            ></path>
          </svg>
        </button>
        <button class="play">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 42 42" width="13" height="13">
            <path
              fill="#fff"
              d="M36.068 20.176l-29-20A1 1 0 1 0 5.5.999v40a1 1 0 0 0 1.568.823l29-20a.999.999 0 0 0 0-1.646z"
            ></path>
          </svg>
        </button>
      </div>
    </header>

    <div class="tracks">
      <div class="recents">
        <figure class="track" v-for="recent in recents" :key="recent.id">
          <img class="track__image" :src="require(`@/assets/tracks/recent/${recent.image}`)" alt="">
          <figcaption>
            <p class="track__title">{{recent.title}}</p>
            <p class="track__duration">{{durationForMin(recent.duration)}}</p>
          </figcaption>
        </figure>
      </div>

      <!-- more -->
      <section class="more-track">
        <div class="more-track__popular">
          <h3>Popular</h3>
          <ul class="popular-list">
            <li v-for="recent in recents" :key="recent.id">
              <figure>
                <img :src="require(`@/assets/tracks/recent/${recent.image}`)" alt="">
                <figcaption class="popular-info">
                  <p>
                    <strong class="popular-title">{{recent.title}}</strong>
                    <span>{{recent.album}}</span>
                  </p>
                  <span>{{durationForMin(recent.duration)}}</span>
                </figcaption>
              </figure>
            </li>
          </ul>
        </div>

        <div class="more-track__playlist">
          <h3>Playlists</h3>
          <ul class="playlist">
            <li v-for="recent in recents" :key="recent.id">
              <figure>
                <img :src="require(`@/assets/tracks/recent/${recent.image}`)" alt="">
                <figcaption>
                  <p>{{recent.album}}</p>
                </figcaption>
              </figure>
            </li>
          </ul>
        </div>
      </section>
      <!-- // more -->

    </div>
  </div>
</template>
<script>
import artistsData from '@/data/artists'
import trackDatas from '@/data/tracks'

export default {
  name: 'Tracks',
  data () {
    return {
      artistId: Number(this.$route.params.id),
      recents: trackDatas[this.$route.params.id].recent
    }
  },
  beforeMount () {
    const rootEl = document.documentElement
    const { left, top } = this.$route.params.position
    const offsetLeft = 100
    const offsetTop = 30
    const x = left - offsetLeft
    const y = top - offsetTop

    rootEl.style.setProperty(
      '--translate-header-image',
      `translate(${x}px, ${y}px) scale(${190 / 240})`
    )
  },
  computed: {
    artist () {
      return artistsData.find(artist => this.artistId === artist.id)
    }
  },
  methods: {
    createUrl (image) {
      return require(`@/assets/${image}`)
    },
    goBack () {
      this.$root.$emit('gotoArtists')
    },
    durationForMin (duration) {
      const min = Math.floor(duration / 60)
      const sec = Math.floor(duration % 60)
      return `${min}:${sec}`
    }
  }
}
</script>
<style lang="scss" scoped>
$easing: cubic-bezier(0.19, 1, 0.22, 1);
:root {
  --translate-header-image: none;
}

.header {
  display: flex;
  justify-content: space-between;
  max-width: 1080px;
  padding: {
    top: 30px;
    left: 20px;
  }

  &__btns {
    flex-grow: 1;
    text-align: right;
  }
}

.btn-forward {
  display: flex;
  justify-content: center;
  align-content: center;
  width: 32px;
  height: 32px;
  margin-right: 20px;
  padding: 0;
  border: 0;
  border-radius: 50%;
  background-color: #353642;
  transition: background-color 0.5s var(--easing-smooth);

  &:hover {
    background-color: #424351;
  }
}

.image-wrapper {
  display: flex;
  flex-grow: 1;
  position: relative;
  width: 240px;
  height: 240px;
  transform-origin: left top;
  animation: move-image 1s $easing;

  .image {
    width: inherit;
    height: inherit;
    margin-right: 30px;
    border-radius: 10px;
    object-fit: cover;
    vertical-align: top;
  }

  &::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 240px;
    height: 100%;
    background: linear-gradient(180deg,transparent,#191a28);
  }
}

@keyframes move-image {
  from {
    transform: var(--translate-header-image);
  }
}

.artist {
  &-name {
    font-size: 32px;
    font-weight: 700;
    line-height: 1.4;
    animation: move-category 1s $easing;
  }
  &-category {
    color: #b2b3c1;
    line-height: 1.2;
    animation: move-category 1.2s $easing;
  }
}

@keyframes move-category {
  from {
    opacity: 0;
    transform: translateX(-100px);
  }

  to {
    opacity: 1;
  }
}

.shuffle,
.play {
  width: 36px;
  height: 36px;
  padding: 0;
  border: 0;
  border-radius: 50%;
  cursor: not-allowed;
}

.shuffle {
  margin-right: 10px;
  background-color: #353642;
}

.play {
  background-color: var(--red);
}

.tracks {
  position: relative;
  bottom: 50px;
  min-height: calc(100vh - 80px);
  padding-left: 100px;
  background-color: #fff;
  border-radius: 15px 15px 0 0;
  color: #333;
  animation: slide-up 1s var(--easing-smooth);
}

@keyframes slide-up {
  from {
    transform: translateY(140px);
  }
}

.recents {
  display: grid;
  grid-template-columns: repeat(50, 160px);
  grid-column-gap: 40px;
  overflow: {
    x: auto;
    y: hidden;
  }
  max-width: 1080px;
  transform: translateY(-50px);
}

.track {
  line-height: 1.4;

  &__image {
    border-radius: 15px;
    box-shadow: 2px 7px 12px -4px rgba(0,0,0,0.43);
    animation: scale-up 1s var(--easing-smooth);
  }

  &__title {
    font-weight: 600;
  }

  &__duration {
    font-size: 14px;
  }
}

@keyframes scale-up {
  from {
    transform: scale(.76);
  }
}

.more-track {
  display: flex;
  justify-content: space-between;
  max-width: 1080px;

  &__playlist {
    width: 354px;
    margin-right: 120px;
  }

  &__popular {
    flex-grow: 1;
    margin-right: 80px;
    // max-width: 980px;
  }

  h3 {
    margin-bottom: 30px;
  }
}

.popular-list {
  $size: 40px;

  li {
    margin-bottom: 20px;
    animation: slide-up 1.5s var(--easing-smooth);

    @for $i from 1 to 5 {
      &:nth-child(#{$i + 1}) {
        animation-delay: $i * .05s;
      }
    }
  }

  img {
    width: $size;
    height: $size;
    margin-right: 20px;
    border-radius: 8px;
  }

  figure {
    display: flex;
  }

  .popular-info {
    display: flex;
    flex-grow: 1;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    line-height: 1.4;
  }

  .popular-title {
    display: block;
    font-size: 14px;
  }
}

.playlist {

  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-column-gap: 20px;
  grid-row-gap: 20px;

  li {
    animation: slide-up 1.5s var(--easing-smooth);

    @for $i from 1 to 5 {
      &:nth-child(#{$i + 1}) {
        animation-delay: $i * .05s;
      }
    }
  }

  figure {
    position: relative;
    overflow: hidden;
    width: 165px;
    height: 90px;
    border-radius: 10px;
    color: #fff;
  }

  figcaption {
    position: absolute;
    bottom: 10px;
    left: 10px;
    font-weight: 600;
  }
}

@keyframes slide-up {
  from {
    transform: translateY(140px);
  }
}
</style>
