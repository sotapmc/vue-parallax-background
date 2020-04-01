<template>
  <div
          ref="block"
          class="header"
          :class="headerCssClass"
          :style="blockElementHeight"
  >
    <div ref="headerBg" class="header__bg" :class="bgCssClass"></div>
    <slot ref="parallaxContent" name="content" :class="contentCssClass" />
  </div>
</template>

<script>
  export default {
    name: "ParallaxBackground",
    data: () => ({
      bgElement: null,
      lFollowX: 0,
      lFollowY: 0,
      mediaQuery: null,
      stuff: "",
      x: 0,
      y: 0
    }),

    props: {
      bgImg: {
        type: String,
        default: ""
      },

      breakpoint: {
        default: "(min-width: 968px)",
        type: String
      },

      direction: {
        type: String,
        default: "up"
      },

      eventType: {
        type: String,
        default: "movemouse"
      },

      friction: {
        type: Number,
        default: 1 / 1000
      },

      gradients: {
        type: Array,
        default: () => []
      },

      height: {
        type: String,
        default: "90"
      },

      speedFactor: {
        default: 0.15,
        type: Number
      }
    },

    computed: {
      blockElementHeight: function() {
        return { height: `${this.height}vh` };
      },

      computedBgImgPath: function() {
        return require(`@/assets/img/${this.bgImg}`);
      },

      directionValue() {
        return this.direction === "down" ? +1 : -1;
      },

      isScrollEvent: function() {
        return this.eventType.toLowerCase() === "scroll";
      },

      bgCssClass: function() {
        return {
          parallax__layer: this.isScrollEvent,
          "parallax__layer--back": this.isScrollEvent
        };
      },

      contentCssClass: function() {
        return {
          parallax__layer: this.isScrollEvent,
          "parallax__layer--base": this.isScrollEvent
        };
      },

      headerCssClass: function() {
        return {
          parallax: this.isScrollEvent,
          header: !this.isScrollEvent
        };
      }
    },

    mounted() {
      const el = this.getTargetElement();
      const vm = this;

      this.bgElement = this.$refs.headerBg;

      this.setBgImageProperty();

      if (this.isScrollEvent) {
        this.initScroll();
      } else {
        el.addEventListener(this.eventType, function(e) {
          vm.handlerMouseMove(e);
        });
      }
    },

    beforeDestroy() {
      window.removeEventListener("scroll", this.scrollHandler, false);
    },

    methods: {
      getTargetElement() {
        return this.isScrollEvent ? window : document.querySelector(".header");
      },

      handlerMouseMove(e) {
        const lMouseX = Math.max(
          -100,
          Math.min(100, window.innerWidth / 2 - e.clientX)
        );

        const lMouseY = Math.max(
          -100,
          Math.min(100, window.innerHeight / 2 - e.clientY)
        );

        this.lFollowX = (20 * lMouseX) / 100;
        this.lFollowY = (10 * lMouseY) / 100;
      },

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
      },

      animateElement() {
        const parentHeight = this.$refs.block.offsetHeight;
        const parallaxHeight = this.$refs.headerBg.offsetHeight;
        const availableOffset = parallaxHeight - parentHeight;
        let animationValue = window.pageYOffset * this.speedFactor;
        if (animationValue <= availableOffset && animationValue >= 0) {
          this.bgElement.style.transform = `translate3d(0, ${animationValue *
          this.directionValue}px ,0)`;
        }
      },

      scrollHandler() {
        window.requestAnimationFrame(() => {
          if (this.isInView(this.bgElement)) {
            this.animateElement();
          }
        });
      },

      isInView(el) {
        let rect = el.getBoundingClientRect();
        return (
          rect.bottom >= 0 &&
          rect.top <=
          (window.innerHeight || document.documentElement.clientHeight)
        );
      },

      setupListener() {
        if (this.mediaQuery.matches) {
          window.addEventListener("scroll", this.scrollHandler, false);
        } else {
          window.removeEventListener("scroll", this.scrollHandler, false);
        }
      },

      initScroll() {
        this.mediaQuery = window.matchMedia(this.breakpoint);
        if (this.mediaQuery) {
          this.mediaQuery.addListener(this.setupListener);
          this.setupListener();
        }
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
  .header__bg {
    position: absolute;
    min-height: 70rem;
    height: 100%;
    background-size: cover; /* Always try to fit elem in box */
    background-position: top; /* Ensure bg img always at top */
    scroll-behavior: smooth;
    transform: scale(1.1);
    z-index: -1;
  }

  .parallax {
    perspective: 1px;
    overflow-x: hidden;
    overflow-y: hidden;
  }

  .parallax__layer {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }

  .parallax__group {
    position: relative;
    height: 100vh;
    transform-style: preserve-3d;
  }

  .parallax__layer--base {
    position: fixed;
    align-content: center;
    justify-content: center;
    margin: 0 auto;
    top: 0;
    left: 0;
    transform: translateZ(0) translate(50%, 70%);
  }

  .parallax__layer--back {
    transform: translateZ(-4px) scale(6);
  }
</style>
