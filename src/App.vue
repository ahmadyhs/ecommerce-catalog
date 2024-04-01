<template>
  <div id="app">
    <ProductCard
      :id="id"
      :nextProduct="this.nextProduct"
      :firstProduct="this.firstProduct"
      :key="id"
    />
  </div>
</template>

<script>
import ProductCard from "./components/ProductCard.vue";

export default {
  name: "App",
  data() {
    return {
      id: 1,
    };
  },
  methods: {
    nextProduct() {
      this.id++;
    },
    firstProduct() {
      this.id = 1;
    },
  },
  components: {
    ProductCard,
  },
};
</script>

<style>
@media only screen and (min-width: 720px) {
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
