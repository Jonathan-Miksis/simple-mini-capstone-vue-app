<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <p v-for="error in errors" v-bind:key="error.id">{{ error }}</p>
    <p>Name: <input type="text" v-model="newProduct.title"></p>
    <p>Description: <input type="text" v-model="newProduct.description"></p>
    <p>Price: <input type="text" v-model="newProduct.price"></p>
    <button v-on:click="createProduct()">Create Product</button>

    <div v-for="product in products" v-bind:key="product.id">
      <p> Name: {{ product.name }}
      <p> Description: {{ product.description }} </p>
      <p> Price: {{ product.price }} </p>
      <p><button v-on:click="showProduct(product)">More Info</button></p>
      <p><button v-on:click="deleteProduct(product)">Delete Product</button>
      <hr>
    </div>
    <dialog id="show-product">
      <form method="dialog">
        <p>Name: <input type="text" v-model="currentProduct.name"></p>
        <p>Description: <input type="text" v-model="currentProduct.description"></p>
        <p>Price: <input type="text" v-model="currentProduct.price"></p>
        <button v-on:click="updateProduct(currentProduct)">Update Product</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome",
      products: [],
      newProduct: {},
      currentProduct: {},
      errors: []
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
        inputName: this.newProduct.title,
        inputDescription: this.newProduct.description,
        inputPrice: this.newProduct.price,
      }).then(response => {
        console.log(response.data);
        this.newProduct = {};
      }).catch(error => {
        console.log(error.response.data.errors);
      });
    },
    showProduct: function(theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      document.querySelector("#show-product").showModal();
      },
    updateProduct: function(theProduct) {
      var productEdit = {
        name: theProduct.name,
        description: theProduct.description,
        price: theProduct.price,
      }
      axios.patch(`http://localhost:3000/products/${theProduct.id}`, productEdit).then(response => { console.log(response.data);
      });
    },
    deleteProduct: function(theProduct) {
      axios.delete(`http://localhost:3000/products/${theProduct.id}`).then(response => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
