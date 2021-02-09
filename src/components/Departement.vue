<template>

  <div class="container">

    <div class="px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h1 class="display-4">Les chiffres du COVID-19 - {{ departement[0].nom }}</h1>
      <p class="lead">Source : {{ departement[0].sourceType }}</p>
      <p class="lead">Date d'actualisation : {{ departement[0].date }}</p>
    </div>

    <div id="chart">
      <div v-if="loading" class="loading">
        <loading-progress :indeterminate=true shape="circle" size="100" />
        <p>Affichage du graphique en cours ...</p>
      </div>
      <apexchart v-else type="area" height="350" :options="graphic.chartOptions" :series="graphic.series"></apexchart>
    </div>

    <div class="row text-center">
      <div class="col">
        <div class="card mb-4 shadow-sm mt-4">
          <div class="card-header">
            <h4 class="my-0 fw-normal">Nombre d’hospitalisations</h4>
          </div>
          <div class="card-body">
            <h1 class="card-title pricing-card-title">{{ departement[0].hospitalises }}</h1>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card mb-4 shadow-sm mt-4">
          <div class="card-header">
            <h4 class="my-0 fw-normal">Nombre de personnes en réanimation</h4>
          </div>
          <div class="card-body">
            <h1 class="card-title pricing-card-title">{{ departement[0].reanimation }}</h1>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card mb-4 shadow-sm mt-4">
          <div class="card-header">
            <h4 class="my-0 fw-normal">Nombre de déces</h4>
          </div>
          <div class="card-body">
            <h1 class="card-title pricing-card-title">{{ departement[0].deces }}</h1>
          </div>
        </div>
      </div>
    </div>

  </div>

</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueApexCharts from 'vue-apexcharts'
import 'vue-progress-path/dist/vue-progress-path.css'
import VueProgress from 'vue-progress-path'

Vue.use(VueApexCharts)
Vue.use(VueProgress)
Vue.component('apexchart', VueApexCharts)

export default {

  data() {
    return {
      departement: [],
      stats: [],
      dateMin: '',
      newHospitalisation: [],
      newReanimation: [],
      dates: [],
      graphic: {},
      loading: true,
      url: "https://coronavirusapi-france.now.sh/"
    }
  },

  mounted() {
    axios.get(this.url + 'LiveDataByDepartement?Departement=' + this.$route.params.nom).then(response => this.departement = response.data.LiveDataByDepartement)
    this.initGraphic()

  },

  methods: {
    formatDate(date) {
      var year = date.getFullYear()
      var getMonth = date.getMonth() + 1
      var month = getMonth.length > 1 ? getMonth : '0' + getMonth
      var day = date.getDate().length > 1 ? date.getDate() : '0' + date.getDate()
      return year + '-' + month + '-' + day
    },

    async initGraphic() {
      for(var i = 1; i < 8; i++) {
        await this.pushAxios(i)
      }
      this.graphic = {
        series: [{
            name: 'Nouvelles hospitalisations',
            data: this.newHospitalisation
          }, {
            name: 'Nouvelles réanimations',
            data: this.newReanimation
        }],
        chartOptions: {
          chart: {
            height: 350,
            type: 'area',
          },
          dataLabels: {
            enabled: false
          },
          stroke: {
            curve: 'smooth'
          },
          xaxis: {
            type: 'datetime',
            categories: this.dates
          },
          tooltip: {
            x: {
              format: 'dd/MM/yy'
            },
          },
        },
      }
      this.loading = false
    },

    async pushAxios(i) {
      var date = this.formatDate(new Date(Date.now() - 86400000*i))
      await this.dates.push(date)
      //this.dateMin = this.graphic.chartOptions.xaxis.categories[0]
      try {
        const response = await axios.get(this.url + 'AllDataByDate?date=' + date)
        var data = await response.data.allFranceDataByDate.find(res => res.nom === this.$route.params.nom)
        this.newHospitalisation.push(data.nouvellesHospitalisations)
        this.newReanimation.push(data.nouvellesReanimations)
      } catch (error) {
        console.log(error)
      }
    }

  },

}
</script>

<style scoped>
  .loading {
    color: green;
    text-align: center;
  }
  .vue-progress-path {
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>
