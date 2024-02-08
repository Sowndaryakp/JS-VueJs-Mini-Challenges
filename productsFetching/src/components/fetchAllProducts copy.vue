<template>
  <div>
    <h1 class="message">{{ msg }}</h1>
    <div class="container">
      <div class="left-card card">
        <h1>Product List</h1>
        <div class="table-container">
          <table class="table">
            <thead>
              <tr>
                <th>ID</th>
                <th>Title</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="products.length === 0">
                <td colspan="2">No products available</td>
              </tr>
              <tr v-for="(product, index) in paginatedProducts" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.title }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="pagination">
          <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
          <span>{{ currentPage }}</span>
          <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
        </div>
      </div>
      <div class="right-card card">
        <h2>Add or Remove Product</h2>
        <div>
          <button @click="addProduct">Add</button>
          <button @click="removeProduct">Remove</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';

const products = ref([]);
const currentPage = ref(1);
const pageSize = 5; // Number of items per page

const msg = 'Welcome to the Product List!';
const totalPages = computed(() => Math.ceil(products.value.length / pageSize));
const paginatedProducts = computed(() => {
  const startIndex = (currentPage.value - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return products.value.slice(startIndex, endIndex);
});

const fetchProducts = async () => {
  try {
    const response = await axios.get('https://dummyjson.com/products');
    products.value = response.data.products;
  } catch (error) {
    console.error('Error fetching products:', error);
  }
};

const addProduct = () => {
  const newProduct = {
    id: Math.floor(Math.random() * 1000), 
    title: `New Product ${products.value.length + 1}` 
  };
  products.value.push(newProduct);
};

const removeProduct = () => {
  if (products.value.length > 0) {
    products.value.pop(); 
  }
};

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value -= 1;
  }
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value += 1;
  }
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.left-card,
.right-card {
  flex: 1;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.right-card {
  max-width: 300px;
}

.right-card h2 {
  margin-bottom: 10px;
}

.right-card button {
  margin-right: 10px;
}

.table-container {
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  padding: 8px 12px;
  border-bottom: 1px solid #ddd;
}

.table th {
  background-color: #f2f2f2;
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-bottom: 10px;
  color: green;
}

.pagination {
  margin-top: 20px;
}

.pagination button {
  margin-right: 10px;
}

.pagination span {
  margin: 0 10px;
}
</style>
