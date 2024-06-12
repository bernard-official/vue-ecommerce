<!-- <template>
    <ShoppingCart />
</template> -->

<template>
    <div class="cart">
      <h3>Cart Items</h3>
        <!-- <ul>
         <li v-for="item in product" :key="product?.id">{{ item?.title }} <button @click="removeCart(item.id)" >Remove</button></li> 
      </ul> -->

        <TransitionGroup appear  name="list" tag="ul">
        <li v-for="item in product" :key="item.id ">
          {{ item?.title }} 
          <button @click="removeCart(item.id)" >Remove</button>
        </li> 
      </TransitionGroup >
       
<!--       
      <TransitionGroup :css="false"
       @before-enter="onBeforeEnter" 
       @enter="onEnter"
       @leave="onLeave"
       tag="ul">
               <li v-for="item in product" :key="product?.id">{{ item?.title }} <button @click="removeCart(item.id)" >Remove</button></li> 
      </TransitionGroup > -->


      <h3>Total no:</h3>{{ product?.length }}
    </div>
</template>
  
<script setup>
import { ShoppingCart } from 'lucide';
import gsap from 'gsap';

import {  ref,computed } from 'vue';

// const product = ref(props.product)
const props = defineProps({
  // items: Array
  product: {
    type: Array,
    required:true,
    default:[]
  }
});


const emits = defineEmits(['removeCart'])

const removeCart = (itemId) => {
    emits('removeCart', itemId) 
}


function onBeforeEnter(el) {
  el.style.opacity = 0
  el.style.height = 0
}

function onEnter(el, done) {
  gsap.to(el, {
    opacity: 1,
    height: '1.6em',
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}

function onLeave(el, done) {
  gsap.to(el, {
    opacity: 0,
    height: 0,
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}
</script>


<style scoped>
.cart {
  border: 1px solid black;
  margin: 1rem ;
}

.list-move,.list-enter-active,.list-leave-active {
 transition: all 0.5s ease;
}

.list-enter-from,.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
