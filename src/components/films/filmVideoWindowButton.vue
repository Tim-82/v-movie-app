<template>
  <div class="window">
      <button
        class="items-center bg-orange-500 text-gray-900 rounded font-semibold px-5 py-3 hover:bg-orange-600 transition ease-in-out duration-150"
        @click="showVideoWindow"
      >
        <i class="far fa-play-circle"></i>
          <span class="ml-2">Play Trailer</span>
      </button>
    <div
      style="background-color: rgba(0, 0, 0, .5);"
      class="fixed top-0 left-0 w-full h-full flex items-center shadow-lg overflow-y-auto"
      v-if="visible"
      ref="popup_wrapper"
    >
      <div class="container mx-auto lg:px-32 rounded-lg overflow-y-auto">
        <div class="bg-gray-900 rounded">
          <div class="flex justify-end pr-4 pt-2">
            <button
              class="text-3xl leading-none hover:text-gray-300"
              @click="visible = !visible"
            >
              &times;
            </button>
          </div>
          <div class="modal-body px-8 py-8">
            <div class="responsive-container overflow-hidden relative" style="padding-top: 56.25%">
              <youtube-video
                class="absolute top-0"
                width="1000"
                height="500"
                :src="`https://www.youtube.com/embed?v=${trailer.results[0].key}`"
              />
              <!-- <iframe
                class="responsive-iframe absolute top-0 left-0 w-full h-full"
                :src="`https://www.youtube.com/embed?v=${trailer}`"
                style="border:0;"
                allow="autoplay; encrypted-media"
                allowfullscreen
              >
              </iframe> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import 'youtube-video-js'

export default {
  name: 'videoWindow',
  components: { },
  props: {
    trailer: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      visible: false
      // image: ''
    }
  },
  methods: {
    showVideoWindow () {
      this.visible = true
    },
    closeVideoWindow () {
      this.visible = false
    }
  },
  mounted () {
    const vm = this
    document.addEventListener('click', function (e) {
      if (e.target === vm.$refs.popup_wrapper) {
        vm.closeVideoWindow()
      }
    })
  }
}
</script>

<style>
.window {
  z-index: 2;
}
</style>
