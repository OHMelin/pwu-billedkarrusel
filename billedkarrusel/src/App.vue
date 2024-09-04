<script>
  export default {
    data() {
      return {
        images: [
          '1.jpg',
          '2.jpg',
          '3.jpg',
          '4.jpg',
          '5.jpg',
          '6.jpg',
        ],
        startIndex: 0,
        maxImages: 3,
        rotationInterval: null,
      };
    },
    computed: {
      callListImages() {
        return this.listImages(this.startIndex, this.maxImages)
      }
    },
    methods: {
      listImages(start, limit) {
        const images = [];
        for (let i = 0; i < limit; i++) {
          const index = (start + i) % this.images.length;
          images.push(this.images[index]);
        }
        return images;
      },
      previous() {
        this.startIndex = (this.startIndex - 1 + this.images.length) % this.images.length
      },
      next() {
        this.startIndex = (this.startIndex + 1) % this.images.length
      },
      autoRotation() {
        this.rotationInterval = setInterval(() => {
          this.next();
        }, 10000)
      },
    },
    mounted() {
      this.autoRotation();
    },
  }
</script>

<template>
  <main>
    <div class="outer-container">
      <button v-on:click="previous()">Previous</button>
      <img v-for="(image, index) in callListImages" :key="index" :src="`/src/assets/images/${image}`"/>
      <button v-on:click="next()">Next</button>
    </div>
  </main>
</template>

<style scoped>
  img {
    width: 100px;
    height: 100px;
  }
  button {
    width: 100px;
  }
  .outer-container {
    display: flex;
    justify-content: center;
    gap: 1rem;
  }
  .img-container {
    height: 100px;
    width: 100px;
    background-color: #101010;
    border-radius: 4px;
  }
</style>
