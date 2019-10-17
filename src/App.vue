<template>
  <div id="app">
    <div id="img-scroll" v-bind:style="{ 'height' : containerHeight + 'px' }">
      <div id="img-stack">
        <div class="img-container" v-for="picture in pictures" v-bind:key="picture.id"
        v-bind:style="getStyle(pictures, picture)">
          <img class="img-element" v-bind:src="`https://via.placeholder.com/${picture.size}`"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'app',
  data: () => ({
    pictures: [
      {
        id: 0,
        size: 600,
        fixed: false,
      },
      {
        id: 1,
        size: 300,
        fixed: true,
      },
      {
        id: 2,
        size: 500,
        fixed: true,
      },
      {
        id: 3,
        size: 200,
        fixed: true,
      },
      {
        id: 4,
        size: 400,
        fixed: true,
      },
    ],
    containerHeight: 0,
  }),
  methods: {
    initContainerHeight(): void{
      const viewPortHeight = Math.max(document.documentElement.clientHeight, window.innerHeight
      || 0);
      // For each pictures to display, we add a viewport height to the scrolling area
      this.pictures.forEach(() => {
        this.containerHeight += viewPortHeight;
      });
    },
    addScrollListener(): void {
      window.addEventListener('scroll', this.handleScroll);
    },
    deleteScrollListener(): void {
      window.removeEventListener('scroll', this.handleScroll);
    },
    handleScroll(event: any): void {
      console.log(window.scrollY);
      // We get the currently moving picture index from the pictures array
      const movingPictureIdx = this.pictures.findIndex(picture => picture.fixed === true) - 1;
      // We get the containers HTMLCollection
      const containers = document.getElementsByClassName('img-container');
      // So we can know what is the top offset of the currently moving picture
      const currentPicOffset = -containers[movingPictureIdx].getBoundingClientRect().top;
      // If the currently moving picture top offset is greater than the viewport...
      if (currentPicOffset > containers[movingPictureIdx].clientHeight
      && movingPictureIdx < this.pictures.length - 1) {
        // ...Then the next picture ain't fixed anymore
        this.pictures[movingPictureIdx + 1].fixed = false;
      }
    },
    getStyle: (pictures: [], picture: any) => {
      const index: number = pictures.length - picture.id;
      return {
        zIndex: index,
        position: picture.fixed ? 'fixed' : 'relative',
      };
    },
  },
  mounted() {
    this.initContainerHeight();
    this.addScrollListener();
  },
  destroyed() {
    this.deleteScrollListener();
  },
});
</script>

<style lang="scss">
#img-stack {
  position: relative;

  .img-container {
    position: fixed;
    height: 100vh;
    width: 100%;
    top: 0;
    left: 0;
    .img-element{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  }
}
</style>
