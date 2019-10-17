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
      },
      {
        id: 1,
        size: 300,
      },
      {
        id: 2,
        size: 500,
      },
      {
        id: 3,
        size: 200,
      },
      {
        id: 4,
        size: 400,
      },
    ],
    containerHeight: 0,
  }),
  methods: {
    initContainerHeight(): void{
      const viewPortHeight = Math.max(document.documentElement.clientHeight, window.innerHeight
      || 0);
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
    },
    getStyle: (pictures: [], picture: any) => {
      const index: number = pictures.length - picture.id;
      return {
        zIndex: index,
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
