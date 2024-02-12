<template>
          <table class="table">
            <thead>
              <tr>
                <th>ID</th>
                <th>Title</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in products" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.title }}</td>
              </tr>
            </tbody>
          </table>
        <h2>Add or Remove Product</h2>
        <div>
          <button @click="addProduct">Add</button>
          <button @click="removeProduct">Remove</button>
        </div>
  </div>
</template>
<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';

const products = ref([]);
const currentPage = ref(1);

const pageSize = 10; // number of items per page
const fetchProducts = async () => {
  try {
    const skip = (currentPage.value - 1) * pageSize;
    const response = await axios.get(`https://dummyjson.com/products?limit=${pageSize}&skip=${skip}`);
    products.value = response.data.products;
  } catch (error) {
    console.error('Error fetching products:', error);
  }
};
fetchProducts();

const addProduct = () => {
  const newProduct = {
    id: Math.floor(Math.random() * 1000), // geenerating  random ID
    title: `New Product ${products.value.length + 1}` // geenerate title
  };
  products.value.push(newProduct); // adding new product to the list
};

const removeProduct = () => {
  if (products.value.length > 0) {
    products.value.pop(); // removing the last product from the list
  }
};
</script>
<style>
</style>