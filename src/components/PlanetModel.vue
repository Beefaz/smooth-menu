<template>
  <div class="iframe-wrapper" ref="imageWrapper"
  >
    <button type="button" class="btn-full-screen" @click="toggleFullScreen">
      <img
          v-if="fullScreenIsOn"
          src="../assets/images/svg/shrink-svgrepo-com.svg"
          alt="expand"
      />
      <img
          v-if="!fullScreenIsOn"
          src="../assets/images/svg/expand-svgrepo-com.svg"
          alt="expand"
      />
    </button>
    <iframe
        src='https://solarsystem.nasa.gov/gltf_embed/2372'
        frameborder='0'
    />
  </div>
</template>

<script>
export default {
  name: 'PlanetModel',
  props: {
    src: String
  },
  data() {
    return {
      fullScreenIsOn: false,
    }
  },
  mounted() {
    document.addEventListener("fullscreenchange", ()=>this.toggleButtonImage());
  },
  methods: {
    toggleFullScreen() {
      document.fullscreenElement !== null
          ? document.exitFullscreen()
          : this.$refs.imageWrapper.requestFullscreen();
    },
    toggleButtonImage() {
      return document.fullscreenElement !== null ? this.fullScreenIsOn = true : this.fullScreenIsOn = false;
    }
  },
}
</script>

<style lang="scss">
.iframe-wrapper {
  position: relative;
  margin-top:50px;

  .btn-full-screen {
    all: unset;
    padding:5px;
    display: flex;
    position: absolute;
    top: 0;
    right: 0;
    cursor: pointer;

    img {
      width: 30px;
      height: 30px;
    }
  }

  iframe {
    width: 100%;
    height: 100%;
    min-height: 50vh;
  }
}
</style>
