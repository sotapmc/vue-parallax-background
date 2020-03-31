<template>
  <header class="header">
    <div id="headerBg" class="header__bg"></div>
    <slot name="content" />
  </header>
</template>

<script>
  export default {
    name: "ParallaxBackground",
    data: () => ({
      stuff: "",
      lFollowX: 0,
      lFollowY: 0,
      x: 0,
      y: 0,
      bgElement: null
    }),

    props: {
      bgImg: {
        type: String,
        default: ""
      },

      gradients: {
        type: Array,
        default: () => []
      },

      friction: {
        type: Number,
        default: 1 / 1000
      }
    },

    computed: {
      computedBgImgPath: function() {
        return require(`@/assets/img/${this.bgImg}`);
      }
    },

    mounted() {
      const el = document.querySelector(".header");
      const vm = this;

      this.bgElement = document.getElementById("headerBg");
      this.setBgImageProperty();

      el.addEventListener("mousemove", function(e) {
        const lMouseX = Math.max(
          -100,
          Math.min(100, window.innerWidth / 2 - e.clientX)
        );

        const lMouseY = Math.max(
          -100,
          Math.min(100, window.innerHeight / 2 - e.clientY)
        );

        vm.lFollowX = (20 * lMouseX) / 100;
        vm.lFollowY = (10 * lMouseY) / 100;
      });
    },

    methods: {
      setBgImageProperty() {
        const hasGradients = this.gradients.length > 0;
        const gradientString = hasGradients ? this.gradients.join() + "," : "";
        this.bgElement.style.backgroundImage = `${gradientString} url('${this.computedBgImgPath}')`;
      },

      translateBackground() {
        this.x += (this.lFollowX - this.x) * this.friction;
        this.y += (this.lFollowY - this.y) * this.friction;
        this.bgElement.style.transform = `translate(${this.x}px,  ${this.y}px) scale(1.1)`;

        window.requestAnimationFrame(this.translateBackground);
      }
    },

    watch: {
      lFollowX() {
        this.translateBackground();
      }
    }
  };
</script>

<style scoped>
  @import "style.css";
</style>
