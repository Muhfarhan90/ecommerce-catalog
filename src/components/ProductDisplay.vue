<script setup>
import "../assets/style/page.css";
import { onMounted, ref } from "vue";
import axios from "axios";

// variabel untuk menyimpan data produk
const product = ref(null);
// fungsi untuk mengambil data produk dari API menggunakan AXIOS
const getProduct = async () => {
  const response = await axios.get("https://fakestoreapi.com/products/1");
  product.value = response.data;
};

// jalankan fungsi getProducts() ketika komponen di-mount
onMounted(() => {
  getProduct();
});
</script>

<template>
  <div class="background">
    <!-- Looping untuk menampilkan data produk -->
    <div v-if="product" :key="product.id" class="product">
      <div class="image-product">
        <img :src="product.image" alt="" />
      </div>
      <div class="product-detail">
        <h1>{{ product.title }}</h1>
        <div class="product-info">
          <p>{{ product.category }}</p>
          <p>{{ product.rating.rate }}/5</p>
        </div>
        <p>{{ product.description }}</p>
        <h2>{{ product.price }}</h2>
        <div class="btn">
          <button>But now</button>
          <button>Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.background {
  background: linear-gradient(to bottom, #fde2ff 60%, #ffffff 40%);
  height: 100vh;
  width: 100%;
  position: absolute;
}

.product {
  display: flex;
  gap: 50px;
  align-items: center;
  width: 80%;
  margin: 0 auto;
  margin-top: 250px;
  padding: 50px 56px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 0 20px #DCDCDC;
}

.product-info {
  display: flex;
  justify-content: space-between;
  font-size: 18px;
}
.image-product {
  width: 400px;
  height: 400px;
  display: flex;
}
h1 {
  color: red;
  font-size: 50px;
}

h2 {
  color: yellow;
}
</style>
