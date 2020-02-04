<template>
  <div class="detailAliment">
    <h1 class="text-center">Détail recherches</h1>
    <button @click="getResults">click</button>
    <ul>
      <li v-for="aliment in aliments">{{ aliment.nom }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      aliments: null
    }
  },
  methods: {
    getResults() {
      fetch('https://localhost:8000/api/products?nom=fromage')
        .then((response) => response.json())
        .then((response) => (this.aliments = response['hydra:member']))
    }
  }
}
</script>
