<template>
  <section :class="`hero is-medium hero-theme-${computedTheme}`">
    <img
      class="hero-bg-img"
      :src="responsiveImage.src"
      :lazy="false"
      :srcset="responsiveImage.srcSet"
    />
    <div class="hero-body">
      <div class="container">
        <h1 class="title animated fadeInUp">
          {{ title }}
        </h1>
        <h2 class="subtitle animated fadeInUp slower">
          {{ subtitle }}
        </h2>
        <br />
        <div
          v-if="$slots.default"
          class="under-subtitle animated fadeInDown slower"
        >
          <slot />
        </div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: 'SiteHero',
  props: {
    title: { type: String, default: '' },
    subtitle: { type: String, default: '' },
    image: { type: String, default: '' },
    color: { type: String, default: '#469af0' },
    theme: { type: String, default: '' }
  },
  computed: {
    responsiveImage() {
      if (this.image.indexOf('/uploads') === 0) {
        return require(`~/assets${this.image}`)
      }
      return { src: this.image, srcSet: '' }
    },
    computedTheme() {
      if (this.theme === '' && this.$siteConfig.hero.theme) {
        return this.$siteConfig.hero.theme
      }
      return this.theme || 'mist'
    }
  }
}
</script>

<style lang="scss" scoped>
.hero {
  margin-top: 52px;
  background-size: cover !important;
  background-position: center;
  text-align: center;
  overflow: hidden;
  position: relative;
}

.title {
  font-weight: 300;
  @media (min-width: 768px) {
    font-size: 3.2rem;
  }
}
.subtitle,
.under-subtitle {
  padding: 0;
  margin: 0;
}
.subtitle {
  font-size: 1rem;
  margin-bottom: 0 !important;
}
.under-subtitle {
  display: inline-block;
  font-size: 0.8rem;
  border-top: 2px solid $primary;
  padding-top: 5px;
}
.opti-image {
  opacity: 0;
}
.opti-image-loaded {
  opacity: 0.12;
  animation: blurIn 4.5s ease;
}
</style>
<style lang="scss">
.hero {
  .hero-bg-img {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    object-fit: cover;
    width: 100%;
    height: 100%;
  }
  .opti-image {
    opacity: 0;
  }
  .opti-image-loaded {
    opacity: 1;
  }
}
.hero-theme-mist {
  .hero-bg-img {
    filter: grayscale(1);
  }
  .opti-image-loaded {
    opacity: 0.12;
    animation: blurInGrayscale 4.5s ease;
  }
}
.hero-theme-dark,
.hero-theme-light {
  &.hero:after {
    content: '';
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.65);
    position: absolute;
  }
  .hero-body {
    position: relative;
    z-index: 2;
  }
}
.hero-theme-dark {
  .title,
  .subtitle,
  .under-subtitle,
  .under-subtitle strong {
    color: white;
  }
}
.hero-theme-light.hero {
  &:after {
    background: rgba(255, 255, 255, 0.6);
  }
  .title,
  .subtitle,
  .under-subtitle,
  .under-subtitle strong {
    text-shadow: 1px 1px 2px white;
  }
}
</style>


<template>
  <div
    :class="{
      'search-wrapper': true,
      active: active,
      transitioning: transitioning,
      inactive: !active && !transitioning
    }"
  >
    <span class="search-bar">
      <input
        id="search-input"
        v-model="query"
        autocomplete="off"
        :class="{
          input: true
        }"
        type="search"
        @keyup.enter="search"
      />

      <ul v-if="matches" class="search-results subtle-box-shadow">
        <div v-if="matches.length">
          <li
            v-for="match in matches"
            :key="match.slug"
            @click="toggleSearchBar()"
          >
            <nuxt-link :to="`/${match.slug}`">
              {{ match.title }}
            </nuxt-link>
            <small class="match-snippet" v-html="match.snippet"></small>
          </li>
        </div>

        <li v-else>
          No results found
          <font-awesome-icon
            icon="times"
            @click="toggleSearchBar()"
          ></font-awesome-icon>
        </li>
      </ul>
    </span>

    <label for="search-input">
      <font-awesome-icon
        icon="search"
        class="search-icon"
        @click="toggleSearchBar()"
      />
    </label>
  </div>
</template>
<script>
export default {
  name: 'Search',
  data() {
    return {
      active: false,
      transitioning: false,
      matches: false,
      haystack: [],
      query: '',
      lastQuery: ''
    }
  },
  methods: {
    toggleSearchBar() {
      this.transitioning = true
      this.active = !this.active
      this.matches = false
      this.query = ''
      this.lastQuery = ''
      setTimeout(() => {
        this.transitioning = false
      }, 500) // Timeout to allow transition to finish
    },
    async search() {
      if (this.lastQuery === this.query) {
        this.toggleSearchBar()
        return
      }
      this.lastQuery = this.query
      if (this.query === '') {
        this.matches = false
        return
      }
      const posts = this.haystack.length
        ? this.haystack
        : await this.$axios.$get('/api/posts.json')
      const matches = posts.filter((match) => {
        return (
          match.content
            .toLowerCase()
            .replace(/#|_|-|~|>|\*|!|\+|`|\||\[|\]|_|:/g, '')
            .indexOf(this.query.toLowerCase()) > -1
        )
      })
      this.matches = matches.map((match) => {
        match.snippet = this.getMatchSnippet(this.query, match.content)
        return match
      })
    },
    getMatchSnippet(query, haystack) {
      const cleanQuery = query.toLowerCase()
      const words = haystack
        .toLowerCase()
        .replace(/#|_|-|~|>|\*|!|\+|`|\||\[|\]|_|:/g, '')
      const pos = words.indexOf(cleanQuery)
      const start = pos > 25 ? pos - 25 : 0
      const end = words.length > pos + 25 ? pos + 25 : words.length
      const prepend = pos > 25 ? '...' : ''
      const append = words.length > pos + 25 ? '...' : ''
      return (
        prepend +
        words
          .slice(start, end)
          .replace(/^. /, '')
          .replace(/ .$/, '')
          .replace(cleanQuery, `<span>${cleanQuery}</span>`)
          .trim() +
        append
      )
    }
  }
}
</script>

<style lang="scss" scoped>
.search-wrapper {
  position: relative;
  input {
    width: 0;
    opacity: 0;
    transition: 0.5s ease width, 0.5s opacity;
  }
  .search-icon {
    position: absolute;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
  }
  &.inactive {
    input {
      padding: 0;
      height: 0;
    }
    .search-icon {
      position: static;
      transform: translateX(-10px);
    }
  }
  &.active {
    input {
      opacity: 1;
      width: 200px;
    }
  }
}
.search-results {
  background: white;
  padding: 10px;
  position: absolute;
  top: 100%;
  right: 0;
  width: 250px;
}
.match-snippet {
  font-size: 0.7em;
  display: block;
}
</style>
<style>
.search-wrapper .match-snippet span {
  background: #eee;
  padding: 0 3px;
  display: inline-block;
}
</style>
