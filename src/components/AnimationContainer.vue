<template>
  <div
      class="animation-container"
      :style="{width: `${spinnerWidth}px`}"
  >
    <div class="x-axis-wrapper"
         v-for="({src, title, id}) in links"
         :key="`img-${id}`"
         :ref="`img-${id}`"
         :style="{
                top: `-${itemSize/2}px`,
                left: `-${itemSize}px`
             }"
    >
      <div class="y-axis-wrapper">
        <div
            class="item-wrapper"
            :style="{
                  width: `${itemSize}px`,
                  height: `${itemSize}px`
                }"
        >
          <img
              class="img"
              :src="src"
              :alt="title"
          >
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'AnimationContainer',
  props: {
    itemSize: {
      type: Number,
      default: 50
    },
    links: {
      type: Array,
      default: () => [],
    },
    selectedId: {
      type: [String, Number]
    },
    spinnerWidth: {
      type: Number,
      default: 100,
    }
  },
  methods: {
    async animationIn(ref) {
      const element = this.$refs[ref][0];

      element.classList.add('move-in');
      return await new Promise((res) => setTimeout(res, 300));
    },

    async animationOut(ref) {
      const element = this.$refs[ref][0];

      element.classList.remove('move-in');
      element.classList.add('move-out');
      await new Promise((res) => setTimeout(res, 300));
      element.classList.remove('move-out');
    },
  },
}
</script>
<style lang="scss" scoped>

.animation-container {
  position: relative;
  height: inherit;
  overflow: hidden;
  margin-left: -1px;
  padding-left: 11px;

  &::before {
    width: 100%;
    height: 100%;
    content: "";
    position: absolute;
    left: 0;
    background-color: #D3D3D3;
    clip-path: ellipse(110% 50.5% at -15% 50%);
  }

  .item-wrapper {
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border-radius: 50vmin;
      box-shadow: -5px 15px 11px -10px #000000;
    }
  }


}

.x-axis-wrapper {
  position: absolute;
  width: 100%;
  height: 100%;

  .y-axis-wrapper {
    width: 100%;
    height: 100%;
  }
}

.x-axis-wrapper.move-in {
  animation: 0.3s move-x-in ease-out both;

  .y-axis-wrapper {
    animation: 0.3s move-y-in ease-in both;
  }
}

.x-axis-wrapper.move-out {
  animation: 0.3s move-x-out ease-in both;

  .y-axis-wrapper {
    animation: 0.3s move-y-out ease-out both;
  }
}

@keyframes move-x-in {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(100%);
  }
}

@keyframes move-x-out {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0%);
  }
}

@keyframes move-y-in {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(50%);
  }
}

@keyframes move-y-out {
  from {
    transform: translateY(50%);
  }
  to {
    transform: translateY(100%);
  }
}

</style>
