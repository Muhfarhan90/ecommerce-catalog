<script setup>
import "../assets/style/page.css";
import { computed, onMounted, ref } from "vue";
import axios from "axios";
import sad from "../../public/sad-face.png";

// Variabel untuk menyimpan data produk
const product = ref(null);
// Variabel untuk menyimpan index produk
const index = ref(1);

// Fungsi untuk mengambil data produk dari API menggunakan AXIOS
const getProduct = async () => {
  const response = await axios.get(`https://fakestoreapi.com/products/${index.value}`);
  product.value = response.data;
  // Hanya menyimpan kategori men's dan women's clothing
  if (
    response.data.category.includes("women's clothing") ||
    response.data.category.includes("men's clothing")
  ) {
    product.value = response.data;
  } else {
    product.value = null;
  }
};
// Fungsi untuk menampilkan produk berikutnya
const nextProduct = async () => {
  index.value++;
  if (index.value > 20) {
    index.value = 1;
  }
  await getProduct();
};
// Menjalankan fungsi getProducts() ketika komponen di-mount / dimmuat
onMounted(() => {
  getProduct();
});

// Computed property untuk menentukan tampilan berdasarkan kategori
const productCategory = computed(() => {
  if (product.value?.category.includes("women's clothing")) return "womens";
  if (product.value?.category.includes("men's clothing")) return "mens";
  return "unavailable";
});
</script>

<template>
  <div :class="['background', productCategory]">
    <!-- Menampilkan data produk jika ada -->
    <div v-if="product" :key="product.id" class="product">
      <div class="image-product">
        <img :src="product.image" alt="" />
      </div>
      <div class="product-detail">
        <h2 id="product-title">{{ product.title }}</h2>
        <div class="product-info">
          <p>{{ product.category }}</p>
          <p>{{ product.rating.rate }}/5</p>
        </div>
        <hr />
        <p id="product-desc">{{ product.description }}</p>
        <hr />
        <h2 id="product-price">${{ product.price }}</h2>
        <div class="btn">
          <button id="btn-buy">But now</button>
          <button id="btn-next" @click="nextProduct">Next Product</button>
        </div>
      </div>
    </div>
    <!-- Menampilkan pesan jika tidak ada data produk -->
    <div v-else class="unavailable">
      <div class="product">
        <img :src="sad" alt="" />
        <div class="message">
          <h2>This product is unavailable to show</h2>
          <button @click="nextProduct" id="btn-next">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
