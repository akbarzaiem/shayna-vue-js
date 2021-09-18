<template>
  <div class="home">
    <Header />
    <Hero />
    <WomanBanner />
    <Instagram />
    <PartnerLogo />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from "@/components/Header.vue";
import Hero from "@/components/Hero.vue";
import WomanBanner from "@/components/WomanBanner.vue";
import Instagram from "@/components/Instagram.vue";
import PartnerLogo from "@/components/PartnerLogo.vue";
import Footer from "@/components/Footer.vue";
export default {
  name: "Home",
  components: {
    // HelloWorld
    Header,
    Hero,
    WomanBanner,
    Instagram,
    PartnerLogo,
    Footer,
  },
};
</script>


