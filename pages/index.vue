<template>
  <span>
    <b-nav class="my-nav" vertical pills v-b-scrollspy:nav-scroller  style="position: fixed; top: 40vh">
      <b-nav-item href="#biography" >
          - BIOGRAPHY
      </b-nav-item>
      <b-nav-item href="#projects">
          - PROJECTS
      </b-nav-item>
      <b-nav-item href="#skilltree">
          - SKILLS
      </b-nav-item>
      <b-nav-item href="#education">
        - EDUCATION
      </b-nav-item>
    </b-nav>
    <!-- <v-navigation-drawer
      fixed
      permanent
      color="transparent"
    >
      <div style="height: 100%" class="d-flex flex-column justify-center align-start">
            <v-btn 
                :rounded="selected_category === 'biography' ? true : false"  
                :plain="selected_category === 'biography' ? false : true"  
                href="#biography"
                @click="check_category('biography')"
              >
                - BIOGRAPHY              
              </v-btn>
            <v-btn 
                :rounded="selected_category === 'projects' ? true : false"  
                :plain="selected_category === 'projects' ? false : true"  
                href="#projects"
                @click="check_category('projects')"
              >
                - PROJECTS              
              </v-btn>
            <v-btn
                :rounded="selected_category === 'skilltree' ? true : false"  
                :plain="selected_category === 'skilltree' ? false : true" 
                href="#skilltree"
                @click="check_category('skilltree')"
              >
                - PROFESSIONAL SKILLS              
              </v-btn>

            <v-btn 
                :rounded="selected_category === 'education' ? true : false"  
                :plain="selected_category === 'education' ? false : true"
                href="#education"
                @click="check_category('education')"
              >
                - EDUCATION              
              </v-btn>
            </div>
    </v-navigation-drawer> -->
    <div class="banner">
      <v-parallax
        id="parallax-id"
        src="/nuxt-web/bg.png"
        style="height: 100%"
      >
        <v-row
        align="center"
        justify="center"
        >
          <v-col
            class="text-center"
            cols="12"
          >
            <h1 class="text-h3 font-weight-thin mb-4">
              - Cheng Yuan Chang -
            </h1>
            <h4 class="subheading">
              Software Engineer!
            </h4>
          </v-col>
        </v-row>
      </v-parallax>
    </div>
    <section id="biography"  class="py-16" style="background-color: black">
      <v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
        <v-row>
          <v-col cols="12" md="6">
              <h1>BIOGRAPHY</h1>
              
              <pre>{{ self_introduction }}</pre>
          </v-col>
          <v-col cols="12" md="6">
            <v-img 
              class="ma-10"
              src="/nuxt-web/bgn.jpg"
            ></v-img>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <section id="projects"  class="my-16">
      <v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
        <h1 class="mb-10">PROJECTS                                                                                                       </h1>
          <v-row>
            <v-col
              cols="12"
              sm="6"
              md="4"
              v-for="card in card_contents" :key="card.title"
            >
              <v-hover
                v-slot="{ hover }"
              >
                <v-card 
                  class="d-flex ma-3 flex-column justify-space-between"
                  height="250"
                  outlined
                  tile
                  :elevation="hover ? 12 : 2"
                >
                  <div class="justified-end">
                    <v-card-title>{{ card.title }}</v-card-title>
                    <v-card-text>{{ card.intro }}</v-card-text>
                      
                    <div class="mx-2">
                      <v-chip
                        small
                        class="ma-1"
                        color="orange"
                        v-for="tag in card.tags"
                        :key="tag"
                      >
                        {{ tag }}
                      </v-chip>
                    </div>
                  </div>

                    <v-card-actions
                      class="mt-4"
                    >
                      <v-btn
                        outlined
                        rounded
                        text
                      >
                        learn more
                      </v-btn>
                    </v-card-actions>
                </v-card>
              </v-hover>
            </v-col>
          </v-row>
      </v-container>
    </section>
    <section id="skilltree" class="my-16" style="background-color: black">
      <client-only placeholder="loading...">
        <skilltree />
      </client-only>
    </section>
    <section id="education"  class="my-16">
      <timeline />
    </section>
  </span>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import Skilltree from '~/components/Skilltree.vue'
import Timeline from '~/components/Timeline.vue'

export default {
  data: function(){
    return {
      self_introduction: `
  Hello! My name is Cheng Yuan Chang. Now, I'm a freshment at graduate institude of Electrical Engineering \
in National Taiwan Universiy, majored in CyberSecurity Program.
   

      `,
      card_contents: [
        { 
          'title': 'FoodExplorer', 'tags': ['React', 'Netlify', 'AWS Lambda', 'Google Maps API'],
          'intro': 'An explorer fooder'
        },
        { 
          'title': 'SurakartaAI', 'tags': ['Deep Reinforcement Learning', 'C++'],
          'intro': 'Surakarta AI is an ref to Alpha go. Wow it s;as djfansiticirfdsf sd'},
        {
          'title': 'Parallel MCTS', 'tags': ['Monte Carlo Tree Search', 'pthread', 'MPI'],
          'intro': 'Surakarta AI is an ref to Alpha go'
        },
        {
          'title': 'BBS Board', 'tags': [],
          'intro': 'Surakarta AI is an ref to Alpha go. Wow it s;as djfansiticirfdsf sd'
        },
        {
          'title': 'GCP Website', 'tags': [],
          'intro': 'Surakarta AI is an ref to Alpha go. Wow it s;as djfansiticirfdsf sd'
        },
        {
          'title': 'Amazon Deploy', 'tags': [],
          'intro': 'Surakarta AI is an ref to Alpha go. Wow it s;as djfansiticirfdsf sd'
        },
      ]
    }
  },
  methods: {
    check_category: function (category) {
      this.selected_category = category
    },
    visibilityChanged: function(isVisible, entry) {
      if (isVisible) {
        this.selected_category = entry.target.id
      }
      console.log(isVisible)
      console.log(entry.target.id)
    }
  },
  components: {
    Logo,
    VuetifyLogo,
    Skilltree,
    Timeline
  }
}
</script>

<style>
.banner {
  /* background-image: url("/nuxt-web/test.jpeg"); */
  height: 100vh;
  /* background-position: center;
  background-repeat: no-repeat;
  background-size: cover; */
}
html {
  scroll-behavior: smooth;
}
.nav-pills > li.active > a.active, .nav-pills > .active > a.hover {
  background-color: #352c2c;
  border-radius: 10px;
}
.nav-pills > li > a  {
  color: gray
}
pre {
    white-space: pre-wrap;       /* Since CSS 2.1 */
    white-space: -moz-pre-wrap;  /* Mozilla, since 1999 */
    white-space: -pre-wrap;      /* Opera 4-6 */
    white-space: -o-pre-wrap;    /* Opera 7 */
    word-wrap: break-word;       /* Internet Explorer 5.5+ */
    color: #f1f3aa;
}
.my-nav {

}
</style>





