<template>
  <div>
    <header class="">
      <div class="container px-4 px-lg-5 my-5">
        <div class="">
          <h1 class="fw-bolder">Search Any Product</h1>
          <input
            class="form-control mr-sm-2"
            type="text"
            placeholder="Search"
            v-model="search"
            v-on:input="filteredList"
          />
          <div
            style="display: flex; justify-content: ; align-items: "
          >
            <select
              class="form-select"
              v-model="sortByOptions.type"
              style="width: 100%; max-width: 600px; margin: 10px"
              @change="sortedLessons"

            >
              <option value="subject">Alphabatically</option>
              <option value="price">Price</option>
              <option value="space">Availability</option>
              <option value="location">location</option>
            </select>

            <select
              class="form-select small"
              style="width: 100%; max-width: 600px; margin: 10px"
              @change="sortedLessons"
              v-model="sortByOptions.direction"
            >
              <option value="ascending">Ascending</option>
              <option value="descending">Descending</option>
            </select>
          </div>
        </div>
      </div>
    </header>

    <section class=" p-5" style="background-color: chocolate;">
      <div class="container-fluid ">
        <div
          class="row  gx-lg-5 row-cols-1  row-cols-xl-4 "
        >
          <div
            class="col mb-5" style="border-radius: 90px;"
            v-for="product in filteredProducts"
            :key="product.id"
          >
            <div class="card h-100">
              <!-- Product image-->
              <img
                class="card-img-top"
                v-bind:src="product.image"
                style="
                  height: 200px;
                  width: 100%;
                  border-bottom: 1px solid white;
                "
              />
              <!-- Product details-->
              <div class="card-body p-4">
                <div class="text-center">
                  <!-- Product name-->
                  <h5 class="fw-bolder" style="border-bottom: 1px solid grey">
                    {{ product.subject }}
                  </h5>
                  <!-- Product price-->
                  <h5 class="fw-normal">Location:</h5>
                  <h5 class="fw-light">{{ product.location }}</h5>
                  <h5 style="margin-top: " class="fw-normal">
                    Spaces: {{ product.space }}
                  </h5>
                  <h5 style="margin-top: " class="fw-semibold">
                    AED {{ product.price }}
                  </h5>
                </div>
              </div>
              <!-- Product actions-->
              <div class="card-footer  bg-transparent">
                <div class="text-center">
                  <button
                    class="btn btn " style="background-color: chocolate; color: aliceblue;"
                    @click="addToCart(product)"
                    v-if="spaceCount(product)"
                  >
                    Add To Cart
                  </button>
                  <button class="btn btn-danger" v-else disabled>
                    Add to Cart
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// import { products } from "./products.js";
import axios from 'axios';

export default {
  data() {
    return {
      products: [],
      filteredProducts: [],
      // products: products,
      search: "",
      sortByOptions: {
        type: "subject",
        direction: "ascending",
        sort: 'ascending',
      },
    };
  },

  methods: {
    filteredList() {
  fetch(`http://localhost:3000/collection/lesson/search?key_word=${this.search}`)
    .then(response => response.json())
    .then(data => {
      this.filteredProducts = data;
    })
    .catch(error => {
      console.error(error);
    });
},
    searchProducts() {
      this.filteredProducts = this.products.filter(product =>
        product.subject.toLowerCase().includes(this.search.toLowerCase())
      );
      this.sortedLessons();
    },

    addToCart(product) {
      this.$emit("addProduct", product);
    },
    spaceCount(product) {
      if (product.space > 0) {
        return true;
      } else {
        return false;
      }
    },
    sortedLessons() {
  switch (this.sortByOptions.type) {
    case 'subject':
      return this.filteredProducts.sort((a, b) => {
        return this.sortByOptions.direction === 'ascending' ? a.subject.toUpperCase() > b.subject.toUpperCase() ? 1 : -1 : a.subject.toUpperCase() < b.subject.toUpperCase() ? 1 : -1;
      });
    case 'location':
      return this.filteredProducts.sort((a, b) => {
        return this.sortByOptions.direction === 'ascending' ? a.location.toUpperCase() > b.location.toUpperCase() ? 1 : -1 : a.location.toUpperCase() < b.location.toUpperCase() ? 1 : -1;
      });
    case 'price':
      return this.filteredProducts.sort((a, b) => {
        return this.sortByOptions.direction === 'ascending' ? a.price - b.price : b.price - a.price;
      });
    case 'space':
      return this.filteredProducts.sort((a, b) => {
        return this.sortByOptions.direction === 'ascending' ? a.space - b.space : b.space - a.space;
      });
    default:
      return this.filteredProducts.sort((a, b) => {
        return this.sortByOptions.direction === 'ascending' ? a.id - b.id : b.id - a.id;
      });
  }
}

  },
  created() {
  axios.get('http://localhost:3000/collection/lesson/search?key_word=' + this.search)
    .then(response => {
      this.products = response.data;
      this.filteredProducts = response.data;
    })
    .catch(error => {
      console.error(error);
    });
},

  computed: {
    // filteredProducts() {
    //   return this.products.filter((product) => {
    //     return (
    //       product.subject.toLowerCase().includes(this.search.toLowerCase()) ||
    //       product.location.toLowerCase().includes(this.search.toLowerCase())
    //     );
    //   });
    // },
  },
};
</script>

<style>
/* Product card */
.card {
  background-color: #b13434;
  border: 1px solid #ccc;
}

.card:hover {
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
  border: 6px solid white;
}

.card .card-img-top {
  object-fit: cover;
  height: 200px;
}

.card .btn-outline-dark {
  color: #000;
  background-color: #fff;
  border-color: black;
}

.card .btn-outline-dark:hover {
  background-color: #000;
  color: #fff;
  border-color: transparent;
}

.card .btn-danger {
  color: #fff;
  background-color: #dc3545;
  border-color: #dc3545;
}

.card .btn-danger:hover {
  background-color: #c82333;
  color: #fff;
  border-color: #bd2130;
}

.card h5 {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: #000;
}

.card p {
  font-size: 0.8rem;
  margin-bottom: 0.25rem;
  color: #555;
}

.card h6 {
  font-size: 1.2rem;
  margin-bottom: 1rem;
  color: #000;
}

.card .text-primary {
  color: #007bff;
}

.card .text-muted {
  color: #6c757d;
}

/* Rounded corners */
.rounded-3 {
  border-radius: 0.5rem !important;
}

/* Shadows */
.shadow {
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1) !important;
}

/* Custom classes */
.title-gradient {
background: linear-gradient(to right, #292f4c, #1b1e32);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
font-size: 2.5rem;
font-weight: 900;
text-align: center;
margin-top: 3rem;
margin-bottom: 3rem;
}

.btn-gradient {
background-image: linear-gradient(to right, #292f4c, #1b1e32);
color: #fff;
border-color: transparent;
transition: all 0.3s ease;
border-radius: 25px;
padding: 10px 25px;
}

.btn-gradient:hover {
background-color: #292f4c;
color: #fff;
border-color: transparent;
}

</style>
