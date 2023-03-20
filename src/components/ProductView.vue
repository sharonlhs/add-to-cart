<template>
  <div class="wrapper">
    <h2>Stay Cool & Comfy Products</h2>
    <ul>
      <li v-for="product in products" :key="product.id">
        <div class="product">
          <span>{{ product.name }} - RM {{ product.price }}</span>
          <button @click="addToCart(product)">Add to cart</button>
        </div>
      </li>
    </ul>
    <h2>Cart</h2>
    <ul class="cart">
      <li v-for="item in cart" :key="item.product.id">
      {{ item.product.name }} - RM {{ item.product.price }} x {{ item.quantity }}
        <button @click="removeFromCart(item)">Delete from cart</button>
      </li>
      <li v-if="cart.length === 0">Your cart is empty</li>
    </ul>
    <div class="total">
      Total amount: RM {{ cartTotal }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Pillow', price: 15},
        { id: 2, name: 'Pillow Case', price: 5.90 },
        { id: 3, name: 'Bed Sheet - Single', price: 35.75 },
        { id: 4, name: 'Bed Sheet - Queen', price: 45.40 },
        { id: 5, name: 'Bed Sheet - King', price: 60 },
      ],
      cart: [],
    };
  },
  methods: {
    addToCart(product) {
      const existingItem = this.cart.find(item => item.product.id === product.id);
      if (existingItem) {
        existingItem.quantity++;
      } else {
        this.cart.push({ product, quantity: 1 });
      }
      localStorage.setItem('cart', JSON.stringify(this.cart)); 
    },
    removeFromCart(item) {
      const index = this.cart.indexOf(item);
      this.cart.splice(index, 1);
      localStorage.setItem('cart', JSON.stringify(this.cart));
    },
  },
  computed: {
    cartTotal() {
      return this.cart.reduce((total, item) => total + (item.product.price * item.quantity), 0);
    },
  },
  mounted() {
    const cart = localStorage.getItem('cart'); //to ensure cart item stay even if user leaves the page or refreshed page
    if (cart) {
      this.cart = JSON.parse(cart);
    }
  },
};
</script>


<style lang="scss">
@import "src/assets/styles.scss";
</style>