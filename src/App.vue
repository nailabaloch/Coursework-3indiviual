<template>
  <div id="app" style="margin: 0">
    <title>Galaxy School</title> 
    <nav class="navbar navbar-expand-lg navbar-light  p-4 m-3" style="background-color: chocolate;">
      <div class="container ">
        <img src="./assets/logo.png">
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto text-white ">
            <li class="nav-item ">
              <a
                class="nav-link-custom nav-link text-white "
                v-on:click="navigateTo('home')"
                aria-current="page"
                style="cursor: pointer"
                v-if="page === 'home'"
                onmouseover="this.style.outline='2px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Home</a
              >
            </li>
            <li class="nav-item ml-5">
              <a
                class="nav-link-custom nav-link text-white"
                v-on:click="navigateTo('home')"
                aria-current="page"
                style="cursor: pointer"
                v-if="page === 'products' || page === 'cart'"
                onmouseover="this.style.outline='2px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Home</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link active text-white"
                v-on:click="navigateTo('products')"
                style="cursor: pointer"
                v-if="page === 'products'"
                onmouseover="this.style.outline='2px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Shop Now</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link text-white"
                v-on:click="navigateTo('products')"
                style="cursor: pointer"
                v-if="page === 'home' || page === 'cart'"
                onmouseover="this.style.outline='2px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Shop Now</a
              >
            </li>
          </ul>
          <form class="d-flex" style="margin-right: 15px">
            <a
              class="btn btn-outline-dark text-white"
              type="submit"
              v-on:click="navigateTo('cart')"
            >
              <i class="bi-cart-fill me-1"></i>
              Cart
              <span class="badge bg-dark text-white ms-1 rounded-pill">{{
                cartCount
              }}</span>
            </a>
          </form>
          <form class="d-flex" v-if="cart.length > 0">
            <a
              class="btn btn-outline-dark"
              type="submit"
              v-on:click="navigateTo('checkout')"
            >
              <i class="bi-cart-fill me-1"></i>
              Checkout
            </a>
          </form>
        </div>
      </div>
    </nav>
    <section class="home-section" v-if="page === 'home'">
      <div
        style="
          align-items: center;
          justify-content: center;
          flex-direction: column;
        "
      >
      <video autoplay muted loop id="myVideo">
  <source src="./assets/mixkit-students-walking-in-a-university-4519-small.mp4" type="video/mp4" class="width-100" >
</video>


      </div>
    </section>
 

    <div id="products">
      <ProductList
        :products="this.products"
        @addProduct="addToCart"
        v-if="page === 'products'"
      ></ProductList>
    </div>

    <div id="cart">
      <Cart
        :cart="cart"
        @remove-from-cart="removeFromCart"
        @reduce-quantity="reduceQuantity"
        v-if="page === 'cart'"
      ></Cart>
    </div>

    <div id="checkout">
      <checkOut
        :checkout="checkOut"
        v-if="page === 'checkout'"
      ></checkOut>
    </div>


  </div>
</template>
<script>



</script>
<script>
import ProductList from "./components/productList.vue";
import Cart from "./components/cartList.vue";
import checkOut from "./components/checkOut.vue";
// import Swal from "sweetalert2";

export default {
  name: "App",
  components: {
    ProductList,
    Cart, checkOut
  },
  data() {
    return {
      page: "home",
      products: [],
      cart: [],
    };
  },

  methods: {
    navigateTo(page) {
      this.page = page;
    },

    addToCart(product) {
      const existingProduct = this.cart.find((item) => item.id === product.id);
  if (existingProduct) {
    existingProduct.cartquantity++;
  } else {
    const cartProduct = { ...product };
    cartProduct.cartquantity = 1;
    this.cart.push(cartProduct);
  }
  product.cartquantity++;
  product.space--;
 
 
    },

    removeFromCart(product) {
      product.cartquantity--;
  product.space--;
  const index = this.cart.findIndex((item) => item.id === product.id);
  if (index !== -1) {
    this.cart.splice(index, 1);
  }

    },

    reduceQuantity(product) {
      product.cartquantity--;
      product.space++;
    },

    cartCounter() {
      if (this.cart.length > 0) {
        return true;
      } else {
        return false;
      }
    },
  },

  computed: {
    cartCount() {
      let count = 0;
      this.cart.forEach((item) => {
        count += item.cartquantity;
      });
      console.log(count);
      return count;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.navbar-logo {
  width: 15%;
  margin-right: 1rem;
}

.home-section {
  background-repeat: no-repeat;
  background-size: cover;
  height: 83.5vh;
}

#products {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  /* height: 83.5vh; */
}
#myVideo {
  min-width: 100%; 

}
</style>
