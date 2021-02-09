<template>

  <div class="container">

    <div class="album py-5" id="france">
      <h1 class="mb-5">Les chiffres du COVID-19 en France</h1>
			<div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
				<div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Nombre d’hospitalisations</h5>
							<p class="card-text">Nombre de personnes actuellement hospitalisée</p>
						</div>
            <h3 class="card-footer bg-warning text-dark mx-2 rounded-pill">{{ stats.hospitalises }}</h3>
					</div>
				</div>
        <div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Nombre de Réa</h5>
							<p class="card-text">Nombre de personnes actuellement en réanimation</p>
						</div>
            <h3 class="card-footer bg-warning text-dark mx-2 rounded-pill">{{ stats.reanimation }}</h3>
					</div>
				</div>
        <div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Nombre total de décès 😥</h5>
							<p class="card-text">Nombre de personnes décédés depuis le début de la pandémie</p>
						</div>
            <h3 class="card-footer bg-dark text-light mx-2 rounded-pill">{{ stats.deces + stats.decesEhpad }}</h3>
					</div>
				</div>
        <div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Nombre totale de guéris</h5>
							<p class="card-text">Nombre de personnes guéris depuis le début de la pandémie</p>
						</div>
            <h3 class="card-footer bg-success text-dark mx-2 rounded-pill">{{ stats.gueris }}</h3>
					</div>
				</div>
        <div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Nombre de cas confirmés</h5>
							<p class="card-text">Nombre de cas confirmés depuis le début de la pandémie</p>
						</div>
            <h3 class="card-footer bg-info text-dark mx-2 rounded-pill">{{ stats.casConfirmes + stats.casConfirmesEhpad }}</h3>
					</div>
				</div>
        <div class="col">
					<div class="card card-stats shadow-sm">
						<div class="card-body">
							<h5 class="card-title">Source des données</h5>
							<p class="card-text">Source des données :<br>{{ stats.sourceType }}</p>
						</div>
           <h3 class="card-footer bg-secondary text-grey mx-2 rounded-pill">{{ stats.date }}</h3>
					</div>
				</div>
			</div>
		</div>

    <div class="album py-5" id="departments">
      <h1 class="mb-5">Les chiffres du COVID-19 par département</h1>
			<div class="row row-cols-1 row-cols-sm-2 row-cols-md-4 g-4">
        <div class="col"  style="margin-bottom: 20px;" v-for="departement in departements" :key="departement.code">
          <div class="card shadow-lg h-100 m-2">
            <div class="card-body">
              <h5 class="card-title">{{ departement.nom }}</h5>
              <p class="card-text">Actuellement hospitalisée: <span class="btn btn-secondary badge">{{ departement.hospitalises }}</span></p>
              <p class="card-text">Actuellement en réanimation: <span class="btn btn-secondary badge">{{ departement.reanimation }}</span></p>
              <router-link :to="{ name: 'Departement', params: { nom: departement.nom } }" class="btn btn-primary btn-plus">Plus d'information</router-link>
            </div>
          </div>
        </div>
			</div>
	  </div>

	</div>

</template>

<script>
import axios from 'axios'

export default {

  data() {
    return {
      stats: [],
      departements: [],
      url: "https://coronavirusapi-france.now.sh/"
    }
  },

  mounted() {
    axios.get(this.url + 'FranceLiveGlobalData').then(response => this.stats = response.data.FranceGlobalLiveData[0])
    axios.get(this.url + 'AllLiveData').then(response => this.departements = response.data.allLiveFranceData)
  }

}
</script>

<style scoped>
  h3 {
    text-align: center;
  }
  .card-stats {
    margin-bottom: 20px;
  }
  .btn-plus:hover {
    background-color: #AA66CC !important;
  }
  .badge {
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>
