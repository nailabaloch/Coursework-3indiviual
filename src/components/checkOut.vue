<template>
    <div
        class="container-fluid "
        style="margin-top: 50px"
      >
        <main>
          <div class="row ">
            <div class="col-md-5">
              <img src="../assets/black.avif" height="500px">
            </div>

            <div class="col-md-7">
              <h4 class="mb-3">Billing Form</h4>
              <form class="needs-validation">
                <div class="row g-3">
                  <div class="col-12">
                    <label for="firstName" class="form-label">Name</label>
                    <input
                      type="text"
                      class="form-control"
                      id="firstName"
                      v-model="order.name"
                      placeholder="Name on ID"
                      required
                    />
                  </div>

                  <div class="col-12">
                    <label for="phone" class="form-label">Phone</label>
                    <input
                      type="number"
                      class="form-control"
                      id="phone"
                      v-model="order.email"
                      placeholder="971-23213213-12"
                      required
                    />
                  </div>

                </div>

                <hr class="my-4" />

                <button
                  class=" btn btn btn-lg text-white"
                  type="submit"
                  v-on:click="onSubmitCheckout"
                  style="margin-bottom: 25px; background-color: chocolate;"
                >
                  Checkout
                </button>
              </form>
            </div>
          </div>
        </main>
      </div>
  </template>

  <script>
  export default {
    name: "checkOut",
    data() {
      return {
      cart: [], // Assuming you have a cart array with items
      order: {
        name: "",
        phone: "",
        gift: false,
      },
      };
    },
    computed: {
      cartTotal() {
  return this.cart.reduce(
    (total, product) => total + product.price * product.space,
    0
  );
},
cartCount() {
  let count = 0;
  this.cart.forEach((item) => {
    count += item.space;
  });
  return count;
},
  },
  methods: {
    navigateTo(page) {
      this.page = page;
    }
  }};
  </script>

  <style>
  /* Add custom styles here */
  </style>
