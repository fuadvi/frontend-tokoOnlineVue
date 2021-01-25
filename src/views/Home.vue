<template>
  <div class="home">
    <HeaderSayna />
    <HeroSection />
    <WomeBanner />
    <Instagram />
    <LogoPatner />
    <FooterSayna />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderSayna from "@/components/HeaderSayna.vue";
import HeroSection from "@/components/HeroSection.vue";
import WomeBanner from "@/components/WomeBanner.vue";
import Instagram from "@/components/Instagram.vue";
import LogoPatner from "@/components/LogoPatner.vue";
import FooterSayna from "@/components/FooterSayna.vue";

export default {
  name: "Home",
  components: {
    HeaderSayna,
    HeroSection,
    WomeBanner,
    Instagram,
    LogoPatner,
    FooterSayna,
  },
};
</script>
