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
          <button id="btn-next">Next Product</button>
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
  box-shadow: 2px 4px 21px #dcdcdc;
}
#product-title {
  margin-bottom: 25px;
  font-size: 28px;
  color: #720060;
}
#product-desc {
  font-size: 20px;
  margin-top: 26px;
  margin-bottom: 62px;
}

#product-price {
  margin-top: 16px;
  margin-bottom: 16px;
}
.product-info {
  display: flex;
  justify-content: space-between;
  font-size: 18px;
  margin-bottom: 12px;
}
.image-product {
  width: 400px;
  height: 400px;
  display: flex;
}

h2 {
  color: #720060;
  font-size: 28px;
}
.btn {
  display: flex;
  gap: 20px;
  justify-content: space-between;
  font-weight: 600;
}

.btn #btn-buy {
  padding: 10px 80px;
  font-size: 20px;
  background-color: #720060;
  color: white;
  border-radius: 10px;
  width: 100%;
}
.btn #btn-next {
  padding: 10px 80px;
  font-size: 20px;
  background-color: white;
  color: #720060;
  border-radius: 10px;
  width: 100%;
  border: 3px solid #720060;
}
</style>
