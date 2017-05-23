<template>
  <div id="vueJustSlider" class="">
      <div class="no-mode-translate-demo-wrapper">
        <button type="button" class="previous" @click="previous">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <path fill="none" d="M0 0h24v24H0z"/>
            <path fill="#333333" d="M18.879 5.249c-.668-.328-1.518-.242-2.107.217L10.4 10.422c-.49.382-.771.957-.771 1.578s.281 1.196.772 1.578l6.372 4.957c.595.462 1.409.558 2.107.217.68-.334 1.12-1.039 1.12-1.797v-9.91c0-.758-.44-1.463-1.121-1.796zM18 16.956l-6.371-4.955L18 7.045v9.911z"/>
            <path fill="#333333" d="M14.625 4.781l-1.25-1.561-10 8c-.237.189-.375.476-.375.78s.138.592.375.781l10 8 1.249-1.562L5.601 12l9.024-7.219z"/>
          </svg>
        </button>
        <transition name="no-mode-translate-fade">
            <div v-for="(item, i) in source" @click="play" v-if="current == i" v-bind:key="i"  :class="to">
              <img :src="item.src" :alt="item.title">
            </div>
        </transition>
        <button type="button" class="next" @click="next">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <path fill="none" d="M0 0h24v24H0z"/>
            <path fill="#333333" d="M14.371 12c0-.621-.281-1.196-.772-1.578L7.228 5.466c-.589-.458-1.439-.544-2.107-.217C4.44 5.582 4 6.287 4 7.045v9.91c0 .758.44 1.463 1.121 1.797.692.338 1.506.25 2.107-.217l6.372-4.957c.49-.382.771-.957.771-1.578zM6 16.955v-9.91L12.372 12 6 16.955z"/>
            <path fill="#333333" d="M20.625 11.22l-10-8-1.25 1.561L18.399 12l-9.024 7.22 1.249 1.562 10-8c.238-.19.376-.478.376-.782s-.138-.591-.375-.78z"/>
          </svg>
        </button>
      </div>
  </div>
</template>

<script>
let timer
export default {
  name: 'just-slider',
  props: {
    source: {
      type: Array,
      default: function(){
        return [{
          src: 'https://files.muzli.space/1ed81c41be64d7d3be0aaf33892f68c3.png',
          title: 'first'
        },{
          src: 'https://files.muzli.space/f01b04d8897c6ac598e3ccc84622440a.png',
          title: 'second'
        },{
          src: 'https://files.muzli.space/e5ae4f0ecfe10d9a3b5a38e7a4380eef.png',
          title: 'third'
        }]
      }
    },
    delay: {
      type: Number,
      default: 6e3
    },

  },
  data() {
    return {
      current: 0,
      to: 'right'
    }
  },
  mounted(){
    this.preload( () => this.play() )
  },
  methods: {
    previous(){
      this.pause()
      if (this.current == 0) {
        this.current = this.source.length - 1
      } else {
        this.current--
      }
      this.to = 'left'
      this.play()
    },
    next() {
      this.pause()
      if (this.current == this.source.length - 1) {
        this.current = 0
      } else {
        this.current++
      }
      this.to = 'right'
      this.play()
    },
    pause() {
      clearInterval(timer)
    },
    play() {
      timer = setInterval(this.next, this.delay)
    },
    preload(cb) {
      if (window.fetch) {
        this.source.forEach( item => {
          let img = new Image()
          img.src = item.src
        })
      }
      cb && cb()
    }
  }
}
</script>

<style lang="scss">
#vueJustSlider {
  min-height: 600px;
  .no-mode-translate-demo-wrapper {
    position: relative;
    display: flex;
    justify-content: space-between;
    .previous,
    .next {
      z-index: 2;
    }
    .previous {
      align-self: flex-start;
    }
    .next {
      align-self: flex-end;
    }
  }
  .no-mode-translate-demo-wrapper div {
    position: absolute;
    width: 100%;
    img {
      margin: 0 auto;
      display: block;
    }
  }

  .no-mode-translate-fade-enter-active, .no-mode-translate-fade-leave-active {
    transition: all 1s;
  }
  .no-mode-translate-fade-enter,
  .no-mode-translate-fade-leave-active {
    opacity: 0;
  }

  .no-mode-translate-fade-enter,
  .no-mode-translate-fade-enter.right,
  .no-mode-translate-fade-leave-active.left {
    transform: translateX(20%);
  }
  .no-mode-translate-fade-leave-active,
  .no-mode-translate-fade-leave-active.right,
  .no-mode-translate-fade-enter.left {
    transform: translateX(-20%);
  }

}
</style>
