<script setup>
useHead({
  title: "15puzzle",
  meta: [
    { charset: "utf-8" },
    { name: "description", description: "Penta's Hubs" },
    { name: "viewport", content: "width=device-width, initial-scale=1" },
  ],
  link: [
    {
      rel: "icon",
      sizes: "32x32",
      type: "image/png",
      href: "/favicon.ico",
    },
  ],
});
</script>
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
