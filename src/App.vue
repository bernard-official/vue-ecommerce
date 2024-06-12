<template>
  <div>
    <div id="nav-section">
      <div>Brand Name</div>
      <div>search input</div>
      <div class="profile">
        <div>Cart</div>
        <div class="avatar">AV</div>
      </div>
    </div>
    <h2>Products</h2>

    <!-- <div class="products-grid">
      <div v-for="product in products" :key="product.id" class="card">
        <div>{{ product.title }}</div>
        <div class="card-image">
          <img :src="product.image" alt="">
        </div>
        <div>
          <p>{{ product.description }}</p>
        </div>
      </div>
    </div> -->

    <!-- <div class="products-grid"> -->
     
        <!-- <Card
          @addToCart="addToCart"
          @removeCart="removeCart"
          v-for="product in products"
          :key="product.id"
          class="card"
          :product="product"
        >
          <template #title>{{ product.title }}</template>

          <img :src="product.image" alt="product image" />

          <template #description>{{ product.description }}</template>
        </Card> -->
     
    <!-- </div> -->

    <TransitionGroup appear name="staggered" tag="ul"
   class="products-grid">
        <Card
          @addToCart="addToCart"
          @removeCart="removeCart"
          v-for="product in products"
          :key="product.id"
          class="card"
          :product="product"
          :style="{'--delay':`${product.id * 0.2}s`}"
        >
          <template #title>{{ product.title }}</template>
          <!-- <template #image> -->
          <img :src="product.image" alt="product image" />
          <!-- </template> -->
          <template #description>{{ product.description }}</template>
        </Card>
    </TransitionGroup>

    <!-- <Cart :items="cartItems"/> -->
    <Cart :product="cartItems" @removeCart="removeCart" />
  </div>
</template>



<script setup>
import { ref, onMounted } from "vue";
import Card from "./components/Card.vue";
import Cart from "./components/Cart.vue";

const products = ref([]);
const cartItems = ref([]);

const url = "https://fakestoreapi.com/products";

const addToCart = (productId) => {
  cartItems.value.push(productId);

  console.log(productId, "product id");
};

const removeCart = (itemId) => {
  const index = cartItems.value.findIndex((product) => {
    return product.id === itemId;
  });
  cartItems.value.splice(index, 1);

  console.log(index, itemId);
};

// const addToCart = (productId)=>{
//   cartItems.value.push(productId)
// }

onMounted(async () => {
  try {
    const response = await fetch(url);
    const data = await response.json();
    products.value = data;
    console.log(products.value);
  } catch (error) {
    console.error("Failed to fetch products:", error);
  }
});
</script>

<style scoped>
#nav-section {
  border: 3px solid black;
  padding: 1rem 0.5rem;
  display: flex;
  justify-content: space-between;
}
#nav-section .profile {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
#nav-section .profile .avatar {
  border: 1px solid black;
  padding: 4px;
  border-radius: 100%;
  width: 100%;
  height: 100%;
  background-color: linear-gradient(to right, #00ffff, red);
}
.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  padding: 1rem;
}
/* .staggered-enter-active,
.staggered-leave-active {
  transition: all 0.3s ease;
}

.staggered-enter-from,
.staggered-leave-to {
  opacity: 0;
  transform: translateX(-300px); 
  /* move in from the left */
/*}

.staggered-move {
  transition: transform 0.3s;

} */

.staggered-move{
  opacity: 0;
  height: 0;
  overflow: hidden;
  transition: opacity 0.3s ease, 
  height 0.3s ease var(--delay);
}
.staggered-enter-active,.staggered-leave-active{
  transition-delay: var(--delay);
  opacity: 1;
  height: 1.6em;
}
.staggered-enter-from,.staggered-leave-to {
  transition-delay: var(--delay);
  opacity: 0;
  height: 0;
}


/* enterFromClass?: string
  enterActiveClass?: string
  enterToClass?: string
  appearFromClass?: string
  appearActiveClass?: string
  appearToClass?: string
  leaveFromClass?: string
  leaveActiveClass?: string
  leaveToClass?: string */
</style>