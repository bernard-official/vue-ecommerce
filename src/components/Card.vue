<template>
  <div class="card">
    <div class="card-title">
      <slot name="title"></slot>
    </div>
    <div class="card-image">
      <slot></slot>
    </div>
    <div class="card-description">
      <slot name="description"></slot>
    </div>
    <div class="cart-buttons">

      <button @click="loadDataToCart" v-if="!lazyItemAddedToCart">Add to cart</button>
      <button @click="removeFromCart" v-else>remove</button>


    <!-- 
      <button @click="loadDataToCart" >Add to cart</button>
      <button @click="removeFromCart" >remove</button> 
    -->
      </div>
  </div>
</template>

<script setup>
// import { defineProps, defineEmits, defineModel } from "vue";
import {  defineModel } from "vue";
import { ref } from "vue";

// Provide the cartItems to the card component since we have the items in the cart in there
// Ineject the cartItems into Card.vue 
// - 3 we will have a computed property that checks if the currentProduct in our prop is in the cartItems
// If it's there return true else return false, if true then you show the remove button else show the add to cart button

// lazy way of displaying add or remove
const lazyItemAddedToCart = ref(false)

const props = defineProps({
  //set the required prop to true for  safe checking 
    product: {
      type:Object,
      required:true,
      default: () => ({})
    }
})

const emits = defineEmits(['addToCart','removeCart'])
// Lazy version
// function removeFromCart(){
//   emits('removeCart', props.product.id)
//   lazyItemAddedToCart.value = false

// }
// // lazy version
// const loadDataToCart = () => {
//   emits('addToCart', props.product)
//   console.log(props.product)
//   lazyItemAddedToCart.value = true
//   }


// normal version
function removeFromCart(){
  emits('removeCart', props.product.id)
}

// normal version 
const loadDataToCart = () => {
  emits('addToCart', props.product)
  console.log(props.product)
  }


  // when you do add-To-Cart -> add-t-o-c-art
// addToCart -> @add-to-cart or addToCart ✅
</script>

<style >
.card {
  border: 1px solid black;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  /* height: 100%; */
}
.card-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}
.card-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
.card-description {
  margin-top: 0.5rem;
}
.cart-buttons{
  display: flex;
  gap: 1rem
}
button {
  position: relative;
  top: 0%;
  margin-top: 0.5rem;
  padding: 0.5rem 1rem;
  cursor: pointer;
}

</style>

<!-- 

<template>
  <div class="card-products-grid">
    <slot></slot>
    <div class="card-image">
      <slot></slot>
    </div>
    <div>
      <slot></slot>
    </div>

    </div> 
 
</template>

<style>
.card-products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  padding: 1rem;
}
.card {
  border: 1px solid black;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.card-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
</style> -->
