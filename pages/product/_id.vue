<template>
  <div>
    <Breadcumb :link="link" :title="title" />
    <div class="card">
      <div class="card-body">
        <div class="row">
          <div class="col-md-6">
            <b-img
              :src="require(`~/assets/img/data/${product.gambar}`)"
              fluid
              alt="Responsive image"
              v-if="product.gambar"
            />
            <b-img
              v-else
              src="../../assets/img/doodle-fruit.png"
              fluid
              alt="Responsive image"
              class="grayscale text-center null-image"
            />
          </div>
          <div class="col-md-6">
            <b-badge
              variant="warning"
              class="best-seller"
              v-if="product.best_seller"
              >Best Seller</b-badge
            >
            <h3 class="grey-text">
              <strong>{{ product.nama }}</strong>
            </h3>
            <h6>
              Price :
              <span class="h5"
                ><span class="small text-muted">IDR</span> {{ product.harga }}
              </span>
              <small class="small text-gray-400">/{{ product.unit }}</small>
            </h6>
            <h6>Description :</h6>
            <small class="text-muted">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
              eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
              enim ad minim veniam, quis nostrud exercitation ullamco
            </small>
            <hr />
            <form v-on:submit.prevent>
              <div class="form-group">
                <label for="">Quantity</label>
                <div class="input-group">
                  <span class="input-group-btn">
                    <button
                      type="button"
                      class="btn btn-default btn-number"
                      v-on:click="decrementQty"
                    >
                      <b-icon-dash></b-icon-dash>
                    </button>
                  </span>
                  <span class="input-container">
                    <input
                      type="text"
                      v-model.number="pesan.quantity"
                      class="form-control input-number"
                      min="0"
                    />
                  </span>
                  <span class="input-group-btn">
                    <button
                      type="button"
                      class="btn btn-default btn-number"
                      v-on:click="pesan.quantity++"
                    >
                      <b-icon-plus></b-icon-plus>
                    </button>
                  </span>
                </div>
              </div>
              <div class="form-group">
                <label for="">Note</label>
                <textarea
                  name=""
                  id=""
                  class="form-control"
                  rows="3"
                  v-model="pesan.note"
                ></textarea>
              </div>
              <button type="sumbit" class="btn btn-success" @click="checkout">
                <b-icon-cart></b-icon-cart> Add to chart
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import { mapGetters, mapActions } from "vuex";

export default {
  props: {},
  validate({ params }) {
    return /^\d+$/.test(params.id);
  },
  data() {
    return {
      pesan: {
        quantity: 0,
      },
      title: "Product Detail",
      link: [
        {
          text: "Home",
          href: "/",
        },
        {
          text: "Products",
          href: "/product",
        },
        {
          text: "",
          active: true,
        },
      ],
    };
  },
  methods: {
    setProductName(data) {
      this.link[2]["text"] = this.product.nama;
    },
    decrementQty() {
      if (this.pesan.quantity > 0) {
        this.pesan.quantity--;
      }
    },
    checkout() {
      if (this.pesan.quantity) {
        this.pesan.products = this.product;
        this.add_cart(this.pesan);
        this.pesan = "";
        this.$router.push({ path: "/cart" });
      } else {
        this.$toasted.error("quantity is required", {
          theme: "toasted-primary",
          position: "top-right",
          duration: 3000,
        });
      }
    },
    ...mapActions({
      get_detail_product: "product/getDetailProduct",
      add_cart: "cart/addCart",
    }),
  },
  mounted() {
    this.get_detail_product(this.$route.params.id);
  },
  computed: {
    ...mapGetters({
      product: "product/getDetailProduct",
    }),
  },
};
</script>

<style  scoped>
.null-image {
  width: 70%;
  margin: 0 auto;
}
.best-seller {
  width: 80px;
}
</style>