<template>
  <div class="product">
    <img
      alt="bg-pattern"
      id="bg"
      v-bind:class="bgClasses"
      src="../assets/bg-pattern.png"
    />

    <div v-if="!error" class="card">
      <div class="image-col">
        <img
          alt="product logo"
          class="product-image"
          v-bind:src="product?.image"
        />
      </div>

      <div class="info-col">
        <h3 v-bind:class="h3Classes">{{ product?.title }}</h3>
        <div class="rating">
          <p class="rating-text">{{ product?.category }}</p>
          <p class="rating-text">{{ product?.rating?.rate }}/5</p>
        </div>
        <hr />

        <p class="desc">{{ product?.description }}</p>
        <hr />

        <h3 v-bind:class="h3Classes" class="price">${{ product?.price }}</h3>

        <div class="button-container">
          <button id="buy-button" v-bind:class="buttonBuyClasses">
            Buy Now
          </button>
          <button
            id="next-button"
            v-if="id === 20"
            v-on:click="firstProduct"
            v-bind:class="buttonNextClasses"
          >
            First Product
          </button>
          <button
            id="next-button"
            v-on:click="nextProduct"
            v-else
            v-bind:class="buttonNextClasses"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>

    <div v-if="error" class="sad-card">
      <img alt="sad-face" id="sad-face" src="../assets/sad-face.png" />

      <div class="unavailable">
        <p>This product is unavailable to show</p>

        <button
          id="unavailable-next-button"
          v-on:click="nextProduct"
          v-bind:class="buttonNextClasses"
        >
          Next Product
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    id: { type: Number, default: 1 },
    nextProduct: { type: Function },
    firstProduct: { type: Function },
  },
  data() {
    return {
      product: {},
      isMen: false,
      isWomen: false,
      error: false,
    };
  },
  computed: {
    bgClasses: function () {
      return {
        bgPatternMen: this.isMen,
        bgPatternWomen: this.isWomen,
        bgError: this.error,
      };
    },
    h3Classes: function () {
      return {
        h3Men: this.isMen,
        h3Women: this.isWomen,
      };
    },
    buttonBuyClasses: function () {
      return {
        buttonBuyMen: this.isMen,
        buttonBuyWomen: this.isWomen,
      };
    },
    buttonNextClasses: function () {
      return {
        buttonNextMen: this.isMen,
        buttonNextWomen: this.isWomen,
        buttonNextError: this.error,
      };
    },
  },
  mounted() {
    fetch(`https://fakestoreapi.com/products/${this.id}`)
      .then((res) => res.json())
      .then((data) => {
        if (data.category === "men's clothing") {
          this.product = data;
          this.image = data.image;
          this.isMen = true;
        } else if (data.category === "women's clothing") {
          this.product = data;
          this.image = data.image;
          this.isWomen = true;
        } else if (
          data.category !== "women's clothing" &&
          data.category !== "men's clothing"
        ) {
          this.error = true;
        }
      })
      .catch((err) => console.log(err));
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
:root {
  --blue: #002772;
  --pink: #fde2ff;
  --black: #1e1e1e;
  --purple: #720060;
  --lightblue: #d6e6ff;
  --darkgray: #3f3f3f;
  --lightgray: #dcdcdc;
  --white: #ffffff;
}

@media only screen and (min-width: 720px) {
  .card {
    width: clamp(700px, 80%, 1000px);
    display: grid;
    grid-template-columns: repeat(12, minmax(0, 1fr));
  }
}

h3 {
  margin: 40px 0 0;
  display: flex;
  text-align: left;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

button {
  all: unset;
  cursor: pointer;
  font-weight: 550;
  border-radius: 0.25em;
  padding-top: 0.5em;
  padding-bottom: 0.5em;
}

#bg {
  position: absolute;
  z-index: 0;
  top: 0px;
  left: 0px;
  width: 100vw;
  overflow: clip;
}

#sad-face {
  width: 100%;
  margin: auto;
}

.sad-card {
  position: relative;
  z-index: 10;
  width: 50%;
  margin-left: 1em;
  margin-right: 1em;
  padding-top: 1em;
  padding-bottom: 1em;
  background-color: var(--white);
  border-radius: 0.5rem;
  filter: drop-shadow(0 10px 8px rgb(0 0 0 / 0.04))
    drop-shadow(0 4px 3px rgb(0 0 0 / 0.1));
}

.h3Men {
  color: var(--blue);
}

.h3Women {
  color: var(--purple);
}

.buttonNextMen {
  color: var(--blue);
  border-color: var(--blue);
}

.buttonNextWomen {
  color: var(--purple);
  border-color: var(--purple);
}

.buttonNextError {
  color: var(--black);
  border-color: var(--black);
}

.buttonBuyMen {
  background-color: var(--blue);
}

.buttonBuyWomen {
  background-color: var(--purple);
}

.product {
  width: 100%;
  display: grid;
  place-items: center;
  margin: auto;
}

.bgPatternMen {
  background-color: var(--lightblue);
}

.bgPatternWomen {
  background-color: var(--pink);
}

.bgError {
  background-color: var(--lightgray);
}

.card {
  position: relative;
  z-index: 10;
  width: auto;
  margin-left: 1em;
  margin-right: 1em;
  padding-top: 1em;
  padding-bottom: 1em;
  color: var(--blue);
  background-color: var(--white);
  border-radius: 0.5rem;
  filter: drop-shadow(0 10px 8px rgb(0 0 0 / 0.04))
    drop-shadow(0 4px 3px rgb(0 0 0 / 0.1));
}

.image-col {
  grid-column: span 4 / span 4;
  display: flex;
}

.info-col {
  grid-column: span 8 / span 8;
  margin-left: 2em;
  margin-right: 2em;
}

.product-image {
  width: 200px;
  margin: auto;
}

.rating {
  display: flex;
  justify-content: space-between;
  color: var(--darkgray);
}

.rating-text {
  margin-bottom: 0;
}

.price {
  margin-top: 0;
  margin-bottom: 1em;
}

.desc {
  color: var(--black);
  text-align: left;
  height: clamp(100px, 5em, auto);
}

.button-container {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  column-gap: 1em;
}

#buy-button {
  width: 100%;
  color: var(--white);
}

#next-button {
  background-color: var(--white);
  border-style: solid;
  border-width: 2px;
}

#unavailable-next-button {
  width: 20em;
  background-color: var(--white);
  border-style: solid;
  border-width: 2px;
}

.unavailable {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
