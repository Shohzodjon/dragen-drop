<template>
  <section class="section">
    <div class="flex mt-4 gap-7 flex-wrap items-start">
      <CardWrapp />
    </div>
  </section>
</template>
<script>
import CardWrapp from "@/pages/CardWrapp.vue";
export default {
  components: {
    CardWrapp,
  },
};
</script>

<style>
@import "@/assets/base.css";
.section {
  background-image: url("@/img/bgimg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  width: 100vw;
  min-height: 100vh;
  padding: 1px;
}
</style>
