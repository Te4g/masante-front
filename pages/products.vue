<template>
  <div id="osef">
    <button @click="getAllProducts">get products !</button>
    <section>
      <div role="tablist">
        <b-card
          v-for="product in products"
          v-bind:key="product.id"
          no-body
          class="mb-1"
        >
          <b-card-header header-tag="header" class="p-1" role="tab">
            <b-button v-b-toggle="product.id" block href="#" variant="info">
              {{ product.nom }} /
              <span class="font-weight-bold"
                >Energie : {{ product.energie }}</span
              ></b-button
            >
          </b-card-header>
          <b-collapse :id="product.id" accordion="my-accordion" role="tabpanel">
            <b-card-body>
              <b-card-text v-for="(key, value) in product"
                ><span class="font-weight-bold">{{ value }}</span> :
                {{ key }}</b-card-text
              >
            </b-card-body>
          </b-collapse>
        </b-card>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: null,
      url: 'https://localhost:8000/api/products/',
      show: false
    }
  },
  methods: {
    getAllProducts() {
      fetch('https://localhost:8000/api/products')
        .then((response) => response.json())
        .then((response) => (this.products = response['hydra:member']))
    }
  }
}
</script>
