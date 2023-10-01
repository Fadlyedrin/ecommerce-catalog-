<template>
    <div :class="getClassForCategory()" class="container">
      <div class="product-card">
        <div v-if="loading" class="loading">
          <div class="spinner">
            <i class="fa fa-spinner fa-spin"></i>
          </div>
        </div>
  
        <div class="product-details" v-else-if="product">
          <div class="product-image">
            <img :src="product.image" alt="image" width="200px" />
          </div>
          <div class="product-detail">
            <h2 class="title">{{ product.title }}</h2>
            <div class="rating">
              <div>{{ product.category }}</div>
              <div class="rate">
                {{ product.rating.rate }}/5
                <span class="full-circle" style="margin-left: 3px"> </span>
                <span class="full-circle"> </span>
                <span class="full-circle"> </span>
                <span class="circle"> </span>
                <span class="circle"> </span>
              </div>
            </div>
            <hr />
            <p class="description" style="min-height: 300px">{{ product.description }}</p>
            <hr />
            <h3 class="price">${{ product.price }}</h3>
            <div class="button">
              <button class="button-buy">Buy Now</button>
              <button class="button-next" @click="getNextProduct">Next Product</button>
            </div>
          </div>
        </div>
  
        <div v-else>
          <p>This product is unavailable to show</p>
          <button class="button-next" @click="getNextProduct">Next Product</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        index: 1,
        product: null, 
        loading: false, 
      };
    },
    methods: {
      getNextProduct() {
        this.index = this.index === 20 ? 1 : this.index + 1;
        this.loading = true; 
        setTimeout(() => {
          fetch(`https://fakestoreapi.com/products/${this.index}`)
            .then((response) => response.json())
            .then((data) => {
              if (data.category === "men's clothing" || data.category === "women's clothing") {
                this.product = data;
              } else {
                this.product = null; 
              }
            })
            .catch((error) => {
              console.error(error);
            })
            .finally(() => {
              this.loading = false; 
            });
        }, 300);
      },
  
      getClassForCategory() {
        if (this.product) {
          if (this.product.category === "men's clothing") {
            return "page-men";
          } else if (this.product.category === "women's clothing") {
            return "page-women";
          }
        }
        return "page-unavailable";
      },
    },
    mounted() {
      this.getNextProduct();
    },
  };
  </script>

<style scoped>
@import "../assets/style/page.css";
</style>
