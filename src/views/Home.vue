<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>My first Vue App!</p>

    <p>Name: <input type="text" v-model="newProduct.title"></p>
    <p>Description: <input type="text" v-model="newProduct.description"></p>
    <p>Price: <input type="text" v-model="newProduct.price"></p>
    <button v-on:click="createProduct()">Create Product</button>

    <div v-for="product in products" v-bind:key="product.id">
      <p> Name: {{ product.name }}
      <p> Description: {{ product.description }} </p>
      <p> Price: {{ product.price }} </p>
      </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProduct: {},
    };
  },
  created: function() {
    this.indexProducts();
  },
  methods: {
    indexProducts: function() {
      axios.get("http://localhost:3000/products").then(response => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct: function() {
      console.log(this.newProduct);
      axios.post("http://localhost:3000/products", {
        inputName: this.newProduct.name,
        inputDescription: this.newProduct.description,
        inputPrice: this.newProduct.price,
      }).then(response => {
        console.log(response.data);
      });
    },
  },
};
</script>
