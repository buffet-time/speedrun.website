<script setup lang="ts">
import SiteFooter from '@/components/blocks/SiteFooter/SiteFooter.vue';
import SiteNavbar from '@/components/blocks/nav/SiteNavbar/SiteNavbar.vue';
</script>

<template>
  <div class="flex flex-col mx-auto h-screen">
    <SiteNavbar />
    <Nuxt />
    <SiteFooter />
  </div>
</template>
