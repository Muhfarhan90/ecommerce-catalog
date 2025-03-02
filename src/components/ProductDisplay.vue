<script setup>
import "../assets/style/page.css";
import { computed, onMounted, ref } from "vue";
import axios from "axios";
import sad from "../../public/sad-face.png";

// Variabel untuk menyimpan data produk
const product = ref(null);
// Variabel untuk menyimpan index produk
const index = ref(1);
// Variabel untuk loading state
const loading = ref(true);
// Fungsi untuk mengambil data produk dari API menggunakan AXIOS
const getProduct = async () => {
  loading.value = true;
  try {
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
  } catch (error) {
    console.log("Error fetching product:", error);
    product.value = null;
  } finally {
    setTimeout(() => {
      loading.value = false; //mematikan loading setelah mendapatkan data
    }, 300);
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

// Computed property untuk tampilan rating circle
const starRating = computed(() => {
  if (!product.value) return [];
  const fullStars = Math.floor(product.value.rating.rate);
  const halfStar = product.value.rating.rate % 1 >= 0.5 ? 1 : 0;
  const emptyStars = 5 - fullStars - halfStar;
  return [
    ...Array(fullStars).fill("fas fa-circle"), // Bintang penuh
    ...Array(halfStar).fill("fas fa-circle-half-stroke"), // Setengah bintang
    ...Array(emptyStars).fill("far fa-circle"), // Bintang kosong
  ];
});
</script>

<template>
  <div :class="['background', productCategory]">
    <!-- Tampilan Loading Skeleton -->
    <div v-if="loading" class="loader"></div>
    <!-- Menampilkan data produk jika ada -->
    <div v-else-if="product" :key="product.id" class="product">
      <div class="image-product">
        <img :src="product.image" alt="" />
      </div>
      <div class="product-detail">
        <h2 id="product-title">{{ product.title }}</h2>
        <div class="product-info">
          <p>{{ product.category }}</p>
          <div class="rating">
            <span class="rating-text">{{ product.rating.rate }}/5</span>
            <span v-for="(star, index) in starRating" :key="index" :class="star"></span>
          </div>
        </div>
        <hr />
        <p id="product-desc">{{ product.description }}</p>
        <hr />
        <h2 id="product-price">${{ product.price }}</h2>
        <!-- Tombol -->
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
