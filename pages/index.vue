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
    <div class="banner">
      <v-parallax
        id="parallax-id"
        src="/bgz.jpg"
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
        <v-row
          justify="end"
        >
          <v-col
            class="text-center scroll"
            cols="12"
          >
            <a class="mylink" href="#biography"><span></span>Scroll</a>
          </v-col>
        </v-row>
      </v-parallax>
    </div>
    <section id="biography"  class="py-16">
      <v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
        <h1>BIOGRAPHY</h1>
        <v-row>
          <v-col cols="12"  lg="6">
              
              <pre>{{ self_introduction_part_one }}</pre>
          </v-col>
          <v-col cols="12" lg="6">
            <v-img 
              class="ma-5"
              src="/me.jpg"
            ></v-img>
          </v-col>
        </v-row>
      </v-container>
    </section>
    <section id="biography2"  class="py-16" style="background-color: black">
      <v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
        <v-row>
          <v-col cols="12" md="6">
            <v-img 
              class="ma-5"
              src="/bga.jpg"
            ></v-img>
          </v-col>
          <v-col cols="12" md="6">
              
              <pre>{{ self_introduction_part_two }}</pre>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <section id="projects"  class="py-16">
      <v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
        <h1 class="mb-10">PROJECTS                                                                                                       </h1>
          <v-row>
            <v-col
              cols="12"
              sm="6"
              md="6"
              v-for="card in card_contents" :key="card.title"
            >
              <v-hover
                v-slot="{ hover }"
              >
                <v-card 
                  class="d-flex ma-3 flex-column justify-space-between"
                  :height="card_height"
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
                        :href="card.link"
                        target="_blank"
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
    <section id="skilltree" class="py-16" style="background-color: black">
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
  computed: {
    card_height() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return 500
        case 'sm':
          return 400
        case 'md':
          return 300
        case 'lg':
          return 300
      }
    }
  },
  data: function(){
    return {
      self_introduction_part_one: `
Hello! My name is Cheng Yuan Chang, a student from Taiwan. Now, I'm a graduate student \
in National Taiwan Universiy, major in Electrical Engineering of Cybersecurity Program.

In college, I majored in Computer Science in National Chiao Tung University. The life and experience there not only \
help me build the strong basic of Computer Science domain knowledge, but also make me get interested in this domain. 

I love learning new knowledge. There is almost infinite knowledge in the domain of Computer Science to learn. \
From web frontend to web backend, I learned how to build a complete, claen, maintainable and scalable website from zero. \
From data pipeline to training model, I learned how to transform gigantic raw data to the valuable prediction model or product. \
There are more knowledge for me to explore, and I'm ready for that.`,

      self_introduction_part_two: `
Besides learning knowledge, I love to communicate, cooperate, and share with others. 

In college, I cooperate with my classmate to develop final projects. During internship, \
I work with my colleague. We discuss the new function's architecture and shared new knowledges with each other. \


In the future, I want to be a software enginner to challenge new coming problems and work with other hard-working colleague.

Last but not least, I hope I can use my expertise to help the world become better.
      `,
      card_contents: [
        { 
          'title': 'FoodExplorer', 'tags': ['React', 'Netlify', 'AWS Lambda', 'Google Maps API'],
          'link': 'https://github.com/CyCTW/FoodExplorer',
          'intro': 'FoodExplorer is a web application that help people build their own custom food map. In this app, \
                    you can store your favorite food and classify them into different category using custom icon. \
                    Try it!',
        },
        { 
          'title': 'SurakartaAI', 'tags': ['Deep Reinforcement Learning', 'C++'],
          'link': 'https://github.com/CyCTW/Surakarta-AI',
          'intro': 'In Surakarta AI, I implement different Reinforcement Learning Algorithm such as Monte Carlo Tree Search \
                    and Deep Q Learning. Also, I implement a Deep Learning Network refer to Alpha-zero paper.\
              '
        },
        {
          'title': 'Parallel MCTS', 'tags': ['Monte Carlo Tree Search', 'pthread', 'openmp'],
          'link': 'https://github.com/cyctw/Parallel-MCTS',
          'intro': 'Parallel MCTS put emphasis on parallelizing the Monte Carlo Tree Search algorithm by exploiting thread \
                    programming such as pthread and openmp. Finally, we successfully improve algorithm\'s \
                    performance at the same time.'
        },
        {
          'title': 'BBS Board', 'tags': ['C++', 'AWS S3', 'Kafka', 'socket'],
          'link': 'https://github.com/CyCTW/NCTU-Network-Programming', 
          'intro': 'By using socket programming, I implement the functionality of BBS Board. Users can post, delete article in BBS board. \
                    Furthermore, some tools like AWS S3 and Kafka are used for better overview.'
        }
        
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
  /* background-image: url("/test.jpeg"); */
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
@media screen and (max-width: 1400px) {
  .my-nav {
    display: none
  }
}
.banner .scroll > .mylink {
  padding-top: 70px;
  position: absolute;
  bottom: 20px;
  left: 50%;
  z-index: 2;
  display: inline-block;
  -webkit-transform: translate(0, -50%);
  transform: translate(0, -50%);
  color:gray;
  font : normal 400 20px/1 'Josefin Sans', sans-serif;
  letter-spacing: .1em;
  text-decoration: none;
  transition: opacity .3s;
}
.banner .scroll > .mylink span{
  position: absolute;
  top: 0;
  left: 50%;
  width: 24px;
  height: 24px;
  margin-left: -12px; 
  border-left: 1px solid #fff;
  border-bottom: 1px solid #fff;
  -webkit-transform: rotate(-45deg);
  transform: rotate(-45deg);
  -webkit-animation: sdb05 1.5s infinite;
  animation: sdb05 1.5s infinite;
  box-sizing: border-box;
}
@-webkit-keyframes sdb05 {
  0% {
    -webkit-transform: rotate(-45deg) translate(0, 0);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    -webkit-transform: rotate(-45deg) translate(-20px, 20px);
    opacity: 0;
  }
}
@keyframes sdb05 {
  0% {
    transform: rotate(-45deg) translate(0, 0);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: rotate(-45deg) translate(-20px, 20px);
    opacity: 0;
  }
}
</style>





