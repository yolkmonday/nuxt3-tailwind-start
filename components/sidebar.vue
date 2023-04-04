<template>
  <div
    id="sidebar"
    :class="data ? 'side_hide' : 'sidebar'"
    class="z-40 fixed min-h-screen border-r bg-primary shadow-xl"
  >
    <nuxt-link to="/" class="px-6 h-20 flex items-center gap-x-1">
      <img src="@/assets/png/logo-white.png" class="h-8 mx-auto" alt="" />
    </nuxt-link>
    <ul v-if="menu.length" class="">
      <li
        v-for="(m, index) in menu"
        :key="index"
        :class="
          m.menu.link === $route.fullPath
            ? 'bg-primary-light text-primary font-bold'
            : 'text-white'
        "
        class="py-4 font-tv"
      >
        <div
          v-if="m.menu_child === null"
          id="menu"
          class="hover:text-[#D1B374]"
        >
          <div id="menu" @click.prevent="dataIndex = ''">
            <nuxt-link :to="m.menu.link" class="flex pl-6 items-center gap-3">
              <i :class="m.menu.icon"></i>
              <span id="menu" class="text-sm">
                {{ m.menu.menu_name }}
              </span>
            </nuxt-link>
          </div>
        </div>
        <div v-else>
          <div
            class="flex items-center justify-between cursor-pointer hover:text-[#D1B374]"
            @click.prevent="dataIndex = dataIndex === index ? '' : index"
          >
            <div class="flex pl-6 items-center gap-3">
              <i :class="m.menu.icon"></i>
              <span class="text-sm">
                {{ m.menu.menu_name }}
              </span>
            </div>
            <div v-if="m.menu_child !== null" class="pr-4">
              <i v-if="dataIndex !== index" class="fas fa-chevron-right"></i>
              <i v-else class="fas fa-chevron-down"></i>
            </div>
          </div>
          <ul :class="index === dataIndex ? 'block' : 'hidden'" class="mt-3">
            <li
              v-for="(c, j) in m.menu_child"
              id="menu"
              :key="j"
              :class="
                c.link === $route.fullPath || includeRoute === c.subRoute
                  ? 'bg-primary-light text-primary font-bold'
                  : 'text-white'
              "
              class="py-3 pl-4 hover:text-[#D1B374] font-tv"
            >
              <nuxt-link :to="c.link" class="flex pl-6 items-center gap-3">
                <i :class="c.icon"></i>
                <span id="menu" class="text-sm">
                  {{ c.menu_name }}
                </span>
              </nuxt-link>
            </li>
          </ul>
        </div>
      </li>
      <li
        :class="
          '/integrasi' === $route.fullPath
            ? 'bg-primary-light text-primary font-bold'
            : 'text-white'
        "
        class="py-4 font-tv"
      >
        <div id="menu" class="hover:text-[#D1B374]">
          <div id="menu">
            <nuxt-link to="/integrasi" class="flex pl-6 items-center gap-3">
              <i class="fas fa-link"></i>
              <span id="menu" class="text-sm"> Integrasi API </span>
            </nuxt-link>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SidebarLayout",
  props: ["data"],
  data() {
    return {
      menu: [],
      dataIndex: "",
      sameSub: false,
      kategori: [],
    };
  },
  computed: {
    includeRoute() {
      return this.$route.fullPath.replace(/[0-9]/g, "");
    },
  },
  watch: {
    $route: {
      handler(v) {
        this.setActive();
      },
    },
  },
  async mounted() {
    const menu = localStorage.getItem("menu");
    this.menu = JSON.parse(menu);
    await this.setActive();
  },
  methods: {
    setActive() {
      if (this.$route.fullPath === this.includeRoute) {
        this.sameSub = true;
      } else {
        this.sameSub = false;
      }
      for (let index = 0; index < this.menu.length; index++) {
        if (this.menu[index].menu_child !== null) {
          for (let j = 0; j < this.menu[index].menu_child.length; j++) {
            if (
              this.menu[index].menu_child[j].link === this.$route.fullPath ||
              this.includeRoute === this.menu[index].menu_child[j]
            ) {
              this.dataIndex = index;
            }
          }
        }
      }
    },
  },
};
</script>

<style>
.sidebar {
  width: 235px;
  transition: width 1s ease-in;
}
.side_hide {
  width: 0px;
  display: none;
  transition: width 1s ease-in-out;
}
</style>
