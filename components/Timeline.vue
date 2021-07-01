<template>
<v-container data-aos="fade-in" data-aos-duration="1500" data-aos-delay="500">
  <h1>EDUCATON & WORK EXPERIENCE</h1>
  <v-timeline>
    <v-timeline-item
      v-for="(year, i) in years"
      :key="i"
      :color="year.color"
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold ${year.color}--text`"
          v-text="year.year"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 ${year.color}--text`">
          {{ year.title }}
        </h2>
        <div>
          {{ year.description }}
        </div>
      </div>
    </v-timeline-item>
  </v-timeline>
</v-container>
</template>

<script>
export default {
  data: () => {
    return {
      years: [
          {
              color: 'cyan',
              year: '2021 / 9',
              title: 'EECS, Graduate at National Taiwan University',
              description: 'In 2021, I got admitted to National Taiwan University, majored in Cybersecurity program \
                of Electrical Engineering. In graduate school, my research area is abount blockchain application.'
          },
          {
              color: 'green',
              year: '2021 / 1',
              title: 'Joinvest Backend Intern',
              description: 'Joinvest is a startup company that aim to provide customers the chance to buy government \
                bond with little money. In this company, I am responsible for developing and maintaining the company\'s\
                website and platform.'
          },
          {
              color: 'red',
              year: '2020 / 7',
              title: 'Trend Micro Data Intern',
              description: 'In Trend Micro, as a data science intern, my job is to help Deep Security team to develop a \
                Machine Learning model for abnormal detection of some specific metrics. '
          },
          {
              color: 'blue',
              year: '2017 / 9',
              title: 'CS, National Chiao Tung University',
              description: 'In 2017, I got admitted to National Chiao Tung University, majored in Computer Science. '
          },
      ]
    }
  }

}

</script>