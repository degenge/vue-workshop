<template>
  <div class="wrapper">
    <!-- Header -->
    <h1 class="header">{{ amount }} Images 📸</h1>

    <!-- Slider -->
    <input class="slider" type="range" min="3" max="60" v-model="amount" />

    <!-- Images -->
    <div class="image__wrapper" v-if="images">
      <a
          class="image__link"
          v-for="image in images"
          :key="image.id"
          :href="image.url"
      >
        <img :src="image.download_url" class="image" />
      </a>
    </div>
  </div>
</template>

<script>
export default {
  created() {
    // This method is executed when page loads
    this.getImages(this.amount);
  },
  data() {
    return {
      amount: 10,
      images: null,
    };
  },
  methods: {
    async getImages(amount) {
      // This method fetches the images
      const response = await fetch(
          `https://picsum.photos/v2/list?limit=${amount}`
      );
      const images = await response.json();
      this.images = images;
    },
  },
  watch: {
    amount() {
      // Everytime the amount changes this code will be executed
      this.getImages(this.amount);
    },
  },
};
</script>

<style scoped>
.wrapper {
  @apply relative container h-full mx-auto px-3 space-y-5;
}

.header {
  @apply mb-4 text-3xl;
}

.slider {
  @apply w-10/12 sm:w-3/6 md:w-72;
}

.image__wrapper {
  @apply grid gap-4 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-6 items-stretch justify-items-stretch;
}
.image__link {
  @apply w-full;
}
.image {
  @apply object-cover h-52;
  @apply w-full shadow-md rounded-2xl;
  @apply border-2 border-transparent;
  @apply hover:border-gray-800 hover:shadow-xl;
}
</style>