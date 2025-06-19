<template>
  <div class="mr-custom mb-5">
    <div class="fade-in">
      <!-- slider -->
      <Slider />

      <!-- product -->
      <div class="container-fluid mt-4 mb-5">
        <div class="mb-4">
          <h5 class="text-uppercase">
            <i class="fa fa-shopping-bag"></i> PRODUK TERBARU
          </h5>
          <hr class="solid" />
        </div>

        <div class="row">
          <div
            class="col-md-3 mt-1 mb-4"
            v-for="product in (products && products.data ? products.data : [])"
            :key="product.id"
          >
            <div class="card h-100 border-0 rounded shadow-sm">
              <div class="card-body">
                <img :src="product.image" class="card-img img-fluid" loading="lazy" />
              </div>
              <div class="card-body bg-light-custom text-center rounded-bottom">
                <div class="mb-2">
                  <h6 class="font-weight-semibold mb-2">
                    <nuxt-link
                      :to="{ name: 'products-slug', params: { slug: product.slug } }"
                      class="text-default"
                    >
                      {{ product.title }}
                    </nuxt-link>
                  </h6>
                  <nuxt-link
                    :to="{ name: 'categories-slug', params: { slug: product.category.slug } }"
                    class="text-muted"
                  >
                    {{ product.category.name }}
                  </nuxt-link>
                </div>
                <h6 class="mb-0 font-weight-semibold">
                  <s class="text-red">Rp. {{ formatPrice(product.price) }}</s> /
                  <strong>{{ product.discount }}%</strong>
                </h6>
                <h5 class="text-success mt-3 mb-0 font-weight-semibold">
                  Rp. {{ formatPrice(calculateDiscount(product)) }}
                </h5>
                <hr />
                <client-only>
                  <vue-star-rating
                    :rating="parseFloat(product.reviews_avg_rating)"
                    :increment="0.5"
                    :star-size="20"
                    :read-only="true"
                    :show-rating="false"
                    :inline="true"
                  />
                  (<strong>{{ product.reviews_count }}</strong> ulasan)
                </client-only>
              </div>
            </div>
          </div>
        </div>

        <div class="row justify-content-center mt-4">
          <div class="text-center">
            <nuxt-link
              :to="{ name: 'products' }"
              class="btn btn-lg btn-warning border-0 rounded shadow-sm"
            >
              LIHAT LEBIH BANYAK
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Slider from "@/components/web/slider.vue";

export default {
  components: {
    Slider,
  },

  head() {
    return {
      title: "MI STORE - Distributor Xiaomi Indonesia Resmi",
      meta: [
        { hid: "description", name: "description", content: "Official Toko Online Penjualan Produk Xiaomi" },
        { hid: "og:title", name: "og:title", content: "MI STORE - Distributor Xiaomi Indonesia Resmi" },
        { hid: "og:site_name", name: "og:site_name", content: "MI STORE" },
        { hid: "og:image", name: "og:image", content: "/images/logo.png" },
        { hid: "og:description", name: "og:description", content: "Produk Xiaomi asli dan bergaransi resmi." },
      ],
    };
  },

  async asyncData({ store, error }) {
    try {
      await store.dispatch("web/product/getProductsData");
    } catch (err) {
      error({ statusCode: 500, message: "Gagal mengambil data produk" });
    }
  },

  computed: {
    products() {
      return this.$store.state.web.product.products;
    },
  },

  methods: {
    formatPrice(value) {
      return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        minimumFractionDigits: 0,
      })
        .format(value)
        .replace("Rp", "")
        .trim();
    },
    calculateDiscount(product) {
      const price = parseFloat(product.price);
      const discount = parseFloat(product.discount);
      return price - (price * discount) / 100;
    },
  },
};
</script>

<style scoped>
.text-red {
  color: #dc3545;
}
</style>
