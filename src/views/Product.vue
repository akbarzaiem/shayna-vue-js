<template>
  <div class="product">
    <Header />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="./"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="img_default" alt="" />
                </div>
                <div
                  class="product-thumbs"
                  v-if="detailPage.galleries.length > 0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :dots="false"
                    :nav="false"
                    :autoplay="true"
                  >
                    <div
                      class="pt"
                      v-for="gambar in detailPage.galleries"
                      :key="gambar.id"
                      @click="changeImage(gambar.photo)"
                      :class="gambar.photo == img_default ? 'active' : ''"
                    >
                      <img :src="gambar.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-justify">
                  <div class="pd-title">
                    <span>{{ detailPage.type }}</span>
                    <h3>{{ detailPage.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <!-- terdapat tag p yang tidak bisa hilang, makanya menggunakan v-html -->
                    <p v-html="detailPage.description"></p>
                    <h4>${{ detailPage.price }}</h4>
                  </div>
                  <div class="quantity">
                    <!-- <router-link to="./cart"> -->
                    <a
                      href="#"
                      @click="
                        addAndSaveCart(
                          detailPage.id,
                          detailPage.name,
                          detailPage.price,
                          detailPage.galleries[0].photo
                        )
                      "
                      class="primary-btn pd-cart"
                    >
                      Add To Cart
                    </a>

                    <!-- </router-link> -->
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->
    <Related />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import carousel from "vue-owl-carousel";
import Related from "@/components/Related.vue";
import axios from "axios";
export default {
  name: "Product",
  components: {
    // HelloWorld
    Header,
    Footer,
    carousel,
    Related,
  },
  data() {
    return {
      img_default: "",
      // thumbs: [
      //   "img/mickey1.jpg",
      //   "img/mickey2.jpg",
      //   "img/mickey3.jpg",
      //   "img/mickey4.jpg",
      // ],
      detailPage: [],
      keranjangUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.img_default = urlImage;
    },
    setDataPicture(data) {
      this.detailPage = data;
      this.img_default = data.galleries[0].photo;
    },

    addAndSaveCart(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct,
      };
      //add
      this.keranjangUser.push(productStored);
      //save
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
    },
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }

    axios
      .get("http://shayna-backend.belajarkoding.com/api/products", {
        params: { id: this.$route.params.id },
      })
      .then((response) => this.setDataPicture(response.data.data));
  },
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 10px;
}
</style>

