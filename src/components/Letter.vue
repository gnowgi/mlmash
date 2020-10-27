<template>
  <section class="letter">
    <div class="letter-container">
      <div class="letter-svg" ref="backgroundLetter" v-html="letterSVG" />
      <div class="letter-svg" ref="letter" v-html="letterSVG" />
    </div>
    <div class="actions">
      <button @click="animate">Play</button>
    </div>
  </section>
</template>

<script>
export default {
  name: "Letter",
  data: () => ({
    animatationTime: 5, // 5s
  }),
  props: {
    letter: String,
  },
  computed: {
    letterSVG: (vm) => require(`!html-loader!@/assets/svgs/${vm.letter}.svg`),
  },
  watch: {
    letter: function() {
      this.animate();
    },
  },
  methods: {
    animatePath(path) {
      const length = path.getTotalLength();
      // Clear any previous transition
      path.style.transition = path.style.WebkitTransition = "none";
      // Set up the starting positions
      path.style.strokeDasharray = length + " " + length;
      path.style.strokeDashoffset = length;
      path.style.strokeWidth = "4";
      path.style.stroke = "#01579b";
      // Trigger a layout so styles are calculated & the browser
      // picks up the starting position before animating
      path.getBoundingClientRect();
      // Define our transition
      path.style.transition = path.style.WebkitTransition = `stroke-dashoffset ${this.animatationTime}s ease-in-out`;
      // Go!
      path.style.strokeDashoffset = "0";
    },
    animate() {
      const bgPaths = this.$refs.backgroundLetter.querySelectorAll("path");
      for (let i = 0; i < bgPaths.length; i++) {
        bgPaths[i].style.stroke = "#607d8b";
      }
      const paths = this.$refs.letter.querySelectorAll("path");
      for (let i = 0; i < paths.length; i++) {
        if (i == 0) {
          this.animatePath(paths[i]);
          continue;
        }
        setTimeout(() => {
          this.animatePath(paths[i]);
        }, this.animatationTime * 1000);
      }
    },
  },
  mounted() {
    this.animate();
  },
};
</script>
<style lang="less">
.letter-svg {
  svg {
    height: 100%;
    max-height: 100%;
    max-width: 100%;
  }
}
.letter-container {
  position: relative;
  height: 50vh;
  .letter-svg {
    position: absolute;
    top: 0;
    left: 0;
  }
}
.actions {
  position: none;
}
</style>