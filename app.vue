<script setup>
useHead({
  title: "Penta Games",
  meta: [
    { charset: "utf-8" },
    { name: "description", description: "Penta Games" },
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
