<template>
  <div @keyup.enter="randomMoment" class="hello" v-bind:style="{ background, color }">
    <div v-if="mobile" class="v-toaster">
        <div class="v-toast v-toast-info more-wtf" v-on:click="randomMoment">
          <a> Click for more WTF</a>
        </div>
      </div>
    <section class="main">
      <Moment :moment="moment" :linkColor="linkColor"/>
      <modal height="auto" name="webpack-what" class="modal">
        <ModalContent />
      </modal>
      <Links :color="color" :show="show"/>
    </section>
  </div>
</template>

<script>
import colorable from 'colorable'
import moments from '../data/data'
import Links from './Links'
import Moment from './Moment'
import ModalContent from './ModalContent'

export default {
  name: 'WebpackWTF',
  components: {
    Links,
    Moment,
    ModalContent
  },
  data: function () {
    return {
      background: '',
      mobile: window.innerWidth <= 768,
      moments,
      moment: {},
      color: '',
      linkColor: '',
      colors: {
        aqua: '#7fdbff',
        blue: '#0074d9',
        lime: '#01ff70',
        navy: '#001f3f',
        teal: '#39cccc',
        olive: '#3d9970',
        maroon: '#85144b',
        purple: '#b10dc9',
        yellow: '#ffdc00',
        gray: '#aaaaaa',
        white: '#ffffff',
        black: '#111111',
        silver: '#dddddd'
      }
    }
  },
  methods: {
    show () {
      this.$modal.show('webpack-what')
    },
    random (arr) {
      return Math.floor(Math.random() * arr.length)
    },
    setProps (allColors, colors, combination) {
      this.moment = this.moments[this.random(this.moments)]
      this.background = colors.hex
      this.color = combination[this.random(combination)].hex
      this.linkColor = combination[this.random(combination)].hex
    },
    randomMoment () {
      const allColors = colorable(this.colors, { compact: true, threshold: 0 })
      const colors = allColors[this.random(allColors)]
      const combination = colors.combinations.filter(a => a.accessibility.aa)

      this.setProps(allColors, colors, combination)
    }
  },
  mounted: function () {
    if (window.innerWidth > 768) {
      this.$toaster.info('Press space for more WTF moments')
    }

    this.randomMoment()
  },
  created: function () {
    window.addEventListener('keyup', (evt) => {
      if (evt.code === 'Space') {
        this.randomMoment()
      }
    })
  }
}
</script>

<style lang="scss">
.hello {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  .more-wtf {
    border-radius: 2px;
    cursor: pointer;
    ::before {
      display: none;
    }

    a {
      color: #fff;
      opacity: 1;
    }
  }

  .main {
    width: 900px;
    max-width: 90%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .v--modal {
    padding: 30px;
    border-radius: 2px;
  }
}
</style>
