<template>
  <div class="model-wrapper" ref="imageWrapper"
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
    <model-viewer
        class="model"
        :src="model"
        :alt="title"
        camera-controls=""
        :camera-orbit="getCameraOrbit"
        :exposure="title === 'Earth' ? 2 : 1"
        interaction-prompt-threshold="1000"
        :min-field-of-view="title === 'Saturn' ? '25deg' : '50deg'"
        :max-field-of-view="title === 'Saturn' ? '75deg' : '150deg'"
        disable-pan=""
        auto-rotate=""
        :ios-src="iosModel"
        reveal="auto"
        preload=""
        ar-status="not-presenting">
    </model-viewer>
  </div>
</template>

<script>
export default {
  name: 'PlanetModel',
  props: {
    title: String,
    model: String,
    iosModel: String,
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
  computed: {
    getCameraOrbit(){
      const orbitParams = {
        Earth: '215deg 0 0',
        Saturn: '0 0 50%'
      }
      return orbitParams[this.title] !== undefined ? orbitParams[this.title] : '';
    }
  }
}
</script>

<style lang="scss" scoped>
.model-wrapper {
  width: 100%;
  height: 100%;

  .btn-full-screen {
    all: unset;
    padding:5px;
    display: flex;
    position: fixed;
    z-index: 2;
    top: 0;
    right: 0;
    cursor: pointer;

    img {
      width: 30px;
      height: 30px;
    }
  }

  .model {
    width: 100%;
    height: 100%;
  }
}
</style>
