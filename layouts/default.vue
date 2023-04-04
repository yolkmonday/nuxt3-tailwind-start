<template>
  <div>
    <div
      v-if="$nuxt.isOffline"
      class="fixed top-0 w-full text-white text-center bg-red-500 py-2 text-xs"
    >
      Tidak terhubung, Periksa Jaringan Anda.
    </div>
    <div class="flex">
      <sidebar :data="fullBars" />
      <div class="w-full" :class="fullBars ? 'ml-0' : 'margin-bar'">
        <div
          class="w-full fixed top-0 left-0 z-10 h-14 bg-white shadow flex items-center px-4 justify-between"
        >
          <div
            :class="!fullBars ? 'bars__' : ''"
            class="text-gray-500 cursor-pointer"
            @click.prevent="setFullBar()"
          >
            <i class="text-primary-dark text-xl fas fa-bars"></i>
          </div>
        </div>
        <div class="md:p-6 lg:p-6 p-0 mt-14">
          <transition name="slide-fade">
            <NuxtPage />
          </transition>
          <div class="h-14"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script >
export default {
  data() {
    return {
      fullBars: false,
    };
  },
  methods: {
    setFullBar() {
      return this.fullBars ? (this.fullBars = false) : (this.fullBars = true);
    },
  },
};
</script>

<style>
html,
body {
  background: rgb(248 250 252);
}

body::-webkit-scrollbar,
.scroll__::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}

body::-webkit-scrollbar-track,
.scroll__::-webkit-scrollbar-track {
  background: #c8d0d4;
}

body::-webkit-scrollbar-thumb,
.scroll__::-webkit-scrollbar-thumb {
  background-color: #cf8f11;
  border-radius: 20px;
  border: 3px solid #cf8f11;
}

.bars__ {
  position: relative;
  left: 230px;
  transition: width 1s ease-in-out;
}

/* mobile */
@media only screen and (max-width: 480px) {
  .margin-bar {
    margin-left: 0px;
  }
}

/* desktop */
@media only screen and (min-width: 1200px) {
  .margin-bar {
    margin-left: 230px;
  }
}

.slide-fade-enter-active {
  transition: all 0.2s ease-in;
}
.slide-fade-leave-active {
  transition: all 0.2s ease-out;
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateY(10px);
  opacity: 0;
}
</style>
