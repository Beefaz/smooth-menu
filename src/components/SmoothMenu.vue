<template>
  <div class="menu" ref="menu">
    <div class="navbar" :style="{height: `${menuHeight}px`}">
      <ul class="navbar__navs">
        <li class="nav"
            :class="{active: state.selectedId === id}"
            v-for="({title, id}) in state.links"
            :key="`link-${id}`"
        >
          <div
              @click="handleCLick(id)"
          >
            {{ title }}
          </div>
        </li>
      </ul>
      <AnimationContainer
          ref="animation-container"
          :item-size="itemSize"
          :links="state.links"
          :selectedId="state.selectedId"
          :spinner-width="spinnerWidth"
      />
    </div>
    <slot/>
  </div>
</template>

<script>
import AnimationContainer from "@/components/AnimationContainer";

export default {
  name: 'SmoothMenu',
  props: {
    links: {
      type: Array,
      default: () => [],
    },
    itemSize: {
      type: Number,
      default: 50
    },
    menuHeight: {
      type: Number,
      default: 200
    },
    spinnerWidth: {
      type: Number,
      default: 100
    }
  },
  components: {
    AnimationContainer,
  },
  data() {
    return {
      state: {
        links: [],
        selectedId: undefined
      }
    }
  },
  beforeMount() {
    this.state.links = this.links.map((item, index) => {
      return {
        id: index,
        ...item
      }
    })
  },
  methods: {
    handleCLick(id) {
      if (this.state.selectedId === id) return;

      if (this.state.selectedId !== undefined) {
        this.$refs['animation-container'].animationOut(`img-${this.state.selectedId}`);
      }

      this.state.selectedId = id;
      this.$refs['animation-container'].animationIn(`img-${id}`);
    },
  }
}
</script>
<style lang="scss" scoped>
.menu {
  display: flex;
  justify-content: space-between;
}

.navbar {
  display: flex;

  .navbar__navs {
    margin: unset;
    padding: 10px;
    flex-direction: column;
    list-style: none;
    background-color: #D3D3D3;
    color: #000000;
    direction: rtl;
    overflow-y: auto;

    &::-webkit-scrollbar {
      width: 10px;
    }

    /* Track */
    &::-webkit-scrollbar-track {
      background: #f1f1f1;
    }

    /* Handle */
    &::-webkit-scrollbar-thumb {
      background: #888;
    }

    /* Handle on hover */
    &::-webkit-scrollbar-thumb:hover {
      background: #555;
    }

    & > * + * {
      margin-top: 10px
    }

    .nav {
      display: flex;
      align-items: flex-start;
      justify-content: flex-end;

      &.active {
        color: #A2A2A2;
      }

      &:hover {
        color: #B2B2B2;
        text-decoration: underline;
        cursor: pointer;
      }
    }
  }

}
</style>
