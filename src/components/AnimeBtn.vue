<template>
  <div class="grid__item theme-1">
    <button class="action"
     ref="bttnBack"
     @click="bttnBackClick">
      <svg class="icon icon--rewind">
        <use xlink:href="#icon-rewind"></use>
      </svg>
    </button>
    <button class="particles-button"
      ref="bttn"
      @click="bttnClick">Send</button>
  </div>
</template>

<script>
import anime from 'animejs/lib/anime.es.js'
import Particles from '@/util/particles.js'
export default {
  mounted () {
    const bttnBack = this.$refs.bttnBack
    const particlesOpts = {}
    particlesOpts.complete = () => {
      if (!this.buttonVisible) {
        anime.remove(bttnBack)
        anime({
          targets: bttnBack,
          duration: 300,
          easing: 'easeOutQuint',
          opacity: 1,
          scale: [0, 1]
        })
        bttnBack.style.pointerEvents = 'auto'
      }
    }

    this.buttonVisible = true
    this.particles = new Particles(this.$refs.bttn, particlesOpts)
  },

  methods: {
    bttnBackClick () {
      if (!this.particles.isAnimating() && !this.buttonVisible) {
        const bttnBack = this.$refs.bttnBack
        anime.remove(bttnBack)
        anime({
          targets: bttnBack,
          duration: 300,
          easing: 'easeOutQuint',
          opacity: 0,
          scale: 0
        })

        bttnBack.style.pointerEvents = 'none'

        this.particles.integrate({
          duration: 800,
          easing: 'easeOutSine'
        })

        this.buttonVisible = !this.buttonVisible
      }
    },

    bttnClick () {
      if (!this.particles.isAnimating() && this.buttonVisible) {
        this.particles.disintegrate()
        this.buttonVisible = !this.buttonVisible
      }
    }
  }
}
</script>
