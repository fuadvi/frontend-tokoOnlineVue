<template>
  <div class="home">
    <HeaderSayna />
    <HeroSection />
    <WomeBanner />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderSayna from "@/components/HeaderSayna.vue";
import HeroSection from "@/components/HeroSection.vue";
import WomeBanner from "@/components/WomeBanner.vue";

export default {
  name: "Home",
  components: {
    HeaderSayna,
    HeroSection,
    WomeBanner,
  },
};
</script>
