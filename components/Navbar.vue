<template>
  <div>
    <b-navbar toggleable="lg" type="light" variant="" class="p-0 mt-2 mb-4">
      <b-navbar variant="faded" type="light">
        <b-navbar-brand href="#">
          <img
            src="../assets/img/brand.png"
            class="d-inline-block align-top brand"
          />
        </b-navbar-brand>
      </b-navbar>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item to="/">Home </b-nav-item>
          <b-nav-item to="/product">Products </b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-nav-item href="#" to="/cart">
            Cart <b-icon-cart></b-icon-cart>
            <!-- <b-icon-book></b-icon-book> -->
            <b-badge variant="success" class="cart-badge">
              {{ orders.length }}
            </b-badge>
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  data() {
    return {
      //orders: [],
    };
  },
  methods: {
    setJumlah(data) {
      this.orders = data;
    },
    ...mapActions({
      get_cart: "cart/getCart",
    }),
  },
  mounted() {
    this.get_cart();
  },
  computed: {
    ...mapGetters({
      orders: "cart/getCart",
    }),
  },
};
</script>