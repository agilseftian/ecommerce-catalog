<template>
  <div id="app">
    <main>
      <div class="card-container" :style="{ background: isClothingCategory ? 'linear-gradient(to bottom, #fde2ff 0%, #fde2ff 50%, #ffff 50%, #ffff 100%)' : 'linear-gradient(to bottom, #f0f0f0 0%, #f0f0f0 50%, #ffff 50%, #ffff 100%)' }">
        <div class="card" :style="{ backgroundColor: isClothingCategory ? 'white' : '#D8D7D7' }">
          <img v-if="isClothingCategory" :src="product ? product.image : ''" :alt="product ? product.title : 'Placeholder'" style="width: 50%" />
          <img v-else src="https://placeholder.com/200x200" alt="Placeholder" style="width: 50%" />
          <div class="container">
            <h1 v-if="isClothingCategory">
              <b>{{ product ? product.title : "" }}</b>
            </h1>
            <h1 v-else><b>Sorry, the product is unavailable</b></h1>
            <div class="text1">
              <p v-if="isClothingCategory">{{ product ? `${product.category} Clothing` : "" }}</p>
              <p v-else></p>
              <div class="rating" v-if="isClothingCategory">
                <p>{{ product ? `${product.rating.rate}/5` : "" }}</p>
              </div>
              <div class="dots" v-if="isClothingCategory">
                <!-- Dots for rating -->
              </div>
            </div>
            <div class="description" v-if="isClothingCategory">
              <p>{{ product ? product.description : "" }}</p>
            </div>
            <div class="price" v-if="isClothingCategory">
              <p class="price-text" :style="{ fontSize: '28px', color: isClothingCategory ? '#002772' : '#720060' }">
                <b>${{ product ? product.price : "" }}</b>
              </p>
            </div>
            <div class="button-container">
              <button v-if="isClothingCategory" class="button">Buy now</button>
              <button v-if="isClothingCategory" class="button2" @click="loadNextProduct">Next product</button>
              <button
                v-else
                class="button2"
                @click="loadNextProduct"
                :style="{
                  background: 'white',
                  border: '3px solid #720060',
                  color: '#720060',
                  padding: '10px 50px',
                  textAlign: 'center',
                  textDecoration: 'none',
                  display: 'inline-block',
                  fontSize: '20px',
                  margin: '4px 2px',
                  cursor: 'pointer',
                }"
              >
                Next product
              </button>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style>
body {
  background: linear-gradient(to bottom, #fde2ff 0%, #fde2ff 50%, #ffff 50%, #ffff 100%);
  margin: 0;
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.card-container {
}

.card {
  padding: 20px;
  background-color: white;
  display: flex;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  width: 100%;
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

.container {
  padding: 20px 16px;
}

.rating {
  margin-left: 240px;
  margin-right: 5px;
}

.text1 {
  display: flex;
  align-items: center;
  border-bottom: 5px solid #dcdcdc;
  margin-bottom: 10px;
  color: black;
}

.dot {
  margin-right: 5px;
  height: 15px;
  width: 15px;
  background-color: white;
  border: 2px solid #720060;
  border-radius: 50%;
  display: inline-block;
}

.dot-colored {
  margin-right: 5px;
  height: 15px;
  width: 15px;
  background-color: #720060;
  border: 2px solid #720060;
  border-radius: 50%;
  display: inline-block;
}

.description {
  border-bottom: 5px solid #dcdcdc;
  margin-top: 10px;
  color: black;
}

.price {
  margin-top: 10px;
}

.price-text {
  font-size: 28px;
  color: #720060;
}

h1 {
  color: #720060;
}

.button-container {
  display: flex;
  gap: 120px;
}

.button {
  background-color: #720060;
  border: none;
  color: white;
  padding: 10px 50px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 20px;
  margin: 4px 2px;
  cursor: pointer;
}
.button2 {
  border: 3px solid #720060;
  color: #720060;
  padding: 10px 50px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 20px;
  margin: 4px 2px;
  cursor: pointer;
}
</style>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      product: null,
      currentIndex: 1,
      isClothingCategory: false,
    };
  },
  mounted() {
    this.fetchProduct();
  },
  methods: {
    fetchProduct() {
      axios
        .get(`https://fakestoreapi.com/products/${this.currentIndex}`)
        .then((response) => {
          if (response.data.category === "men's clothing" || response.data.category === "women's clothing") {
            this.product = response.data;
            this.isClothingCategory = true;
          } else {
            this.isClothingCategory = false;
          }
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    loadNextProduct() {
      if (this.currentIndex < 20) {
        this.currentIndex++;
        this.fetchProduct();
      }
    },
  },
};
</script>
