<script>
  export default {
    data() {
      return {
        images: [
          {path: '0.jpg', comments: ['abc0', 'def']},
          {path: '1.jpg', comments: ['abc1', 'def']},
          {path: '2.jpg', comments: ['abc2', 'def']},
          {path: '3.jpg', comments: ['abc3', 'def']},
          {path: '4.jpg', comments: ['abc4', 'def']},
          {path: '5.jpg', comments: ['abc5', 'def']},
        ],
        currentIndex: 0,
        maxImages: 3,
        rotationInterval: null,
      };
    },
    computed: {
      callListImages() {
        return this.listImages(this.currentIndex, this.maxImages)
      }
    },
    methods: {
      // Carousel
      listImages(start, limit) {
        const images = [];
        for (let i = 0; i < limit; i++) {
          const index = (start + i) % this.images.length;
          images.push(this.images[index]);
        }
        return images;
      },
      previous() {
        this.currentIndex = (this.currentIndex - 1 + this.images.length) % this.images.length
      },
      next() {
        this.currentIndex = (this.currentIndex + 1) % this.images.length
      },
      autoRotation() {
        this.rotationInterval = setInterval(() => {
          this.next();
        }, 10000)
      },

      // Comments
      addComment() {
        if (this.comment.trim()) {
          this.images[this.currentIndex].comments.push(this.comment);
          this.comment = '';
          this.saveComments();
        }
      },
      saveComments() {
        localStorage.setItem('images', JSON.stringify(this.images));
      },
      loadComments() {
        const savedImages = localStorage.getItem('images');
        if (savedImages) {
          this.images = JSON.parse(savedImages)
        }
      },
    },
    created() {
      this.loadComments();
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
      <img v-for="(image, index) in callListImages" :key="index" :src="`/src/assets/images/${image.path}`"/>
      <button v-on:click="next()">Next</button>
    </div>
    <div class="comment-container">
      <p>Add comment for middle picture</p>
      <input v-model="comment" placeholder="Your comment"></input>
      <button v-on:click="addComment()">Submit</button>
      <p>Current comments:</p>
      <ul>
        <li v-for="(comment, index) in images[this.currentIndex].comments" :key="index">{{ comment }}</li>
      </ul>
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
  .comment-container {
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
  .img-container {
    height: 100px;
    width: 100px;
    background-color: #101010;
    border-radius: 4px;
  }
</style>
