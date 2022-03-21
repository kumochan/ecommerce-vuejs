<template>
  <!-- <Navbar
    :cartCount="cartCount"
    @resetCartCount="resetCartCount"
    v-if="!['Signup', 'Signin'].includes($route.name)"
  /> -->
  <div>
    <div id="app">
      <router-view />
    </div>
  </div>
  <!-- <Footer v-if="!['Signup', 'Signin'].includes($route.name)" /> -->
</template>

<style>
@import "./assets/admindek/plugins/bootstrap/css/bootstrap.min.css";
@import "./assets/admindek/plugins/waves/css/waves.min.css";
@import "./assets/admindek/icon/feather/css/feather.css";
@import "./assets/admindek/css/font-awesome-n.min.css";
@import "./assets/admindek/plugins/chartist/css/chartist.css";
@import "./assets/admindek/css/style.css";
@import "./assets/admindek/css/widget.css";
</style>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
export default {
  data() {
    return {
      baseURL: "https://limitless-lake-55070.herokuapp.com/",
      //baseURL: "http://localhost:8080/",
      products: null,
      categories: null,
      key: 0,
      token: null,
      cartCount: 0,
    };
  },

  components: { Footer, Navbar },
  methods: {
    async fetchData() {
      // fetch products
      await axios
        .get(this.baseURL + "product/")
        .then((res) => (this.products = res.data))
        .catch((err) => console.log(err));

      //fetch categories
      await axios
        .get(this.baseURL + "category/")
        .then((res) => (this.categories = res.data))
        .catch((err) => console.log(err));

      //fetch cart items
      if (this.token) {
        await axios.get(`${this.baseURL}cart/?token=${this.token}`).then(
          (response) => {
            if (response.status == 200) {
              // update cart
              this.cartCount = Object.keys(response.data.cartItems).length;
            }
          },
          (error) => {
            console.log(error);
          }
        );
      }
    },
    resetCartCount() {
      this.cartCount = 0;
    },
  },
  mounted() {
    this.token = localStorage.getItem("token");
    this.fetchData();
  },
};
</script>

<style>
html {
  overflow-y: scroll;
}
</style>
