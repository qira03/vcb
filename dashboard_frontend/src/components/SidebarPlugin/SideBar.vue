<template>
  <div class="sidebar"
       :data-color="activeColor"
       :data-image="backgroundImage"
       :data-background-color="backgroundColor"
       :style="sidebarStyle">

    <div class="logo">
      <a href="https://www.creative-tim.com" class="simple-text logo-mini" target="_blank">
        <div class="logo-img">
          <img :src="logo">
        </div>
      </a>
      <a href="https://www.creative-tim.com" class="simple-text logo-normal" target="_blank">
        <template v-if="$route.meta.rtlActive">{{rtlTitle}}</template>
        <template v-else>{{title}}</template>
      </a>
      <!-- <div class="navbar-minimize">
        <md-button id="minimizeSidebar" class="md-round md-just-icon md-transparent" @click="minimizeSidebar">
          <i class="material-icons text_align-center visible-on-sidebar-regular">more_vert</i>
        <i class="material-icons design_bullet-list-67 visible-on-sidebar-mini">view_list</i>
        </md-button>
      </div> -->
    </div>
    <div class="sidebar-wrapper" ref="sidebarScrollArea">
      <slot></slot>
      <md-list class="nav">
        <slot name="links">
          <sidebar-item v-for="(link, index) in sidebarLinks"
                        :key="link.name + index"
                        :link="link">

            <sidebar-item v-for="(subLink, index) in link.children"
                          :key="subLink.name + index"
                          :link="subLink">
            </sidebar-item>
          </sidebar-item>
        </slot>
      </md-list>
    </div>
  </div>
</template>
<script>
export default {
  name: "sidebar",
  props: {
    title: {
      type: String,
      default: "VRCB Unicauca"
    },
    rtlTitle: {
      type: String,
      default: "توقيت الإبداعية"
    },
    activeColor: {
      type: String,
      default: "primary",
      validator: value => {
        let acceptedValues = [
          "",
          "primary",
          "azure",
          "green",
          "orange",
          "danger",
          "rose"
        ];
        return acceptedValues.indexOf(value) !== -1;
      }
    },
    backgroundImage: {
      type: String,
      default: "./img/antorcha_azul.jpg"
    },
    backgroundColor: {
      type: String,
      default: "blue",
      validator: value => {
        let acceptedValues = ["","blue","black", "white", "red"];
        return acceptedValues.indexOf(value) !== -1;
      }
    },
    logo: {
      type: String,
      default: "./img/escudo-blanco.png"
    },
    sidebarLinks: {
      type: Array,
      default: () => []
    },
    autoClose: {
      type: Boolean,
      default: true
    }
  },
  provide() {
    return {
      autoClose: this.autoClose
    };
  },
  methods: {
    minimizeSidebar() {
      if (this.$sidebar) {
        this.$sidebar.toggleMinimize();
      }
    }
  },
  computed: {
    sidebarStyle() {
      return {
        backgroundImage: `url(${this.backgroundImage})`
      };
    }
  },
  beforeDestroy() {
    if (this.$sidebar.showSidebar) {
      this.$sidebar.showSidebar = false;
    }
  },
  // Función que verifica que si no ha iniciado sesión, lo devuelve
  beforeCreate(){
    if(this.$store.getters.getToken == '')
    {
      this.$router.push('/login');
    }
  }
};
</script>
<style>
@media (min-width: 992px) {
  .navbar-search-form-mobile,
  .nav-mobile-menu {
    display: none;
  }
}
</style>
