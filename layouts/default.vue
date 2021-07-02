<template>
  <v-app dark>
    <v-main>
      <nuxt />
    </v-main>
    
    <v-footer>
      <v-col
        cols="12"
      >
        <h4 class="text-h5 text-center">
          Contact me for more information!
        </h4>
      </v-col>
      <v-col
        cols="12"
      >
        <v-card
          flat
          tile
          class="lighten-1 white--text text-center"
        >
          <v-card-text>
            <v-btn
              v-for="icon in icons"
              :key="icon.attr"
              class="mx-4 white--text"
              icon
              :href="icon.link"
              target="_blank"
            >
              <v-icon size="24px">
                {{ icon.attr }}
              </v-icon>
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
      
      <span>Cheng Yuan Chang&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  head: {
    bodyAttrs: {
      'id': 'nav-scroller',
      'style': 'position: relative'
    },
  },
  data () {
    return {
      icons: [
        {
          attr: 'mdi-facebook',
          link: 'https://www.facebook.com/NCTU193'
        },
        {
          attr: 'mdi-email',
          link: 'mailto:0cyctwn@gmail.com'
        },
        {
          attr: 'mdi-linkedin',
          link: 'https://www.linkedin.com/in/cyctw/'
        },
        {
          attr: 'mdi-rss',
          link: 'https://cyctw-cs.medium.com/'
        }
      ],
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
      scroll: false
    }
  }
}
</script>
