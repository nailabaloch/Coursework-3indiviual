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
                      v-model="user.name"
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
                      v-model="user.email"
                      placeholder="971-23213213-12"
                      required
                    />
                  </div>

                </div>

                <hr class="my-4" />

                <button  class="w-100 btn btn-primary btn-lg" 
                value="Checkout"
                 @click="submitCheckout"  
                 style="margin-bottom: 25px">
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
      user: {
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
    },
    submitCheckout() {
      // Update the space property of lessons in the cart
      this.cart.forEach((item) => {
        const lessonIndex = this.lessons.findIndex(
          (lesson) => lesson.id === item.id
        );
        if (lessonIndex !== -1) {
          this.lessons[lessonIndex].space += 1;
        }
      });

      // Create the order object with updated space property
      const order = {
        checkoutName: this.user.name,
        checkoutemail: this.user.email,
        checkoutaddress: this.user.address,
        checkoutstate: this.user.state,
        checkoutphone: this.user.phone,

        cartProduct: this.cart,
      };

      // Send the order to the server
      fetch("http://localhost:3000/collection/order", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        mode: "cors",
        cache: "no-store",
        body: JSON.stringify(order),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log("Order submitted:", data);
          alert(data.message);
          // Handle the response data here (e.g., show a success message)
        })
        .catch((error) => {
          console.log("Error submitting order:", error);
          // Handle the error here (e.g., show an error message)
        });

      // Reset the cart
      this.cart = [];
      // Reset the user details
      this.user = {
        name: "",
        phone: "",
        method: "Home",
      };
    },
  }};
  </script>

  <style>
  /* Add custom styles here */
  </style>
