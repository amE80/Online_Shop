<template>
  <div>
    <transition name="fade">
      <!-- <div class="popup" v-show="!activePopup"></div> -->
    </transition>
    {{ SubMenus }}
    <div
      class="total"
      :class="{
        scroll: scrollPosition >= 160,
        container: scrollPosition < 160,
      }"
    >
      <div class="container">
        <div class="right">
          <ul class="menu r">
            <!-- <li
              class="subbox"
              @mouseover="mouseover"
              @mouseleave="mouseleave"
              :class="{ active: !activePopup }"
            >
              <a href="#" class="sub">دسته بندی</a>
              <transition name="fade">
                <sub-menu v-show="!activePopup"></sub-menu>
              </transition>
            </li> -->
            <li>
              <router-link class="router forget" to="/">خانه</router-link>
            </li>
            <li
              v-for="sub in SubMenus"
              v-show="sub.group == null"
              :key="sub.id"
            >
              <a class="sub-list">{{ sub.name }}</a>

              <div class="sub-product">
                <ul>
                  <li>پرتقال</li>
                  <li>سیب</li>
                  <li>هلو</li>
                  <li>شلیل</li>
                  <li>آلو</li>
                </ul>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import SubMenu from "../SubMenus/SubMenu.vue";
export default {
  // components: {
  //   SubMenu,
  // },
  data() {
    return {
      activePopup: true,
      scrollPosition: 0,
      isSticky: false,
    };
  },

  created() {
    this.$store.dispatch("GetSubMenuFromServer");

    // this.$store.dispatch("ShowOrderRows");
    window.addEventListener("scroll", this.updateScroll);
  },
  unmounted() {
    window.removeEventListener("scroll", this.updateScroll);
  },
  methods: {
    mouseover() {
      this.activePopup = false;
    },
    mouseleave() {
      this.activePopup = true;
    },
    updateScroll() {
      this.scrollPosition = window.scrollY;
      if (this.scrollPosition >= 100) {
        this.isSticky = true;
      } else {
        this.isSticky = false;
      }
    },
  },
  computed: {
    SubMenus() {
      return this.$store.getters.GetSubMenu;
    },
    // userOrders() {
    //   return this.$store.getters.GetUserShopCart;
    // },
    // IsAuthenticated() {
    //   return this.$store.getters.IsAuthenticated;
    // },
    // getCountOrder(){
    //   return this.$store.getters.getCountOrder;
    // }
  },
};
</script>


<style lang="scss" scoped>
.total {
  padding-bottom: 1rem;
  width: 100%;
  border-bottom: 1px solid black;
  background-color: #373737;
  @media only screen and (max-width: 1100px) {
    border-bottom: 0px;
  }
}
.menu {
  display: flex;
  list-style: none;
  text-decoration: none;
  direction: rtl;
  height: auto;
  margin-top: 10px;
  @media only screen and (max-width: 1100px) {
    display: none;
  }
}
.r li {
  padding: 10px 0;
  margin-right: 80px;
}
.fa {
  font-size: 35px;
  margin-top: -5px;
  margin-right: 10px;
  transition: all 0.3s;
}
.sub-list {
  cursor: pointer;
}
.sub-product {
  background-color: white;
  position: absolute;
  width:10%;
  border-radius: 5px;
  height: auto;
}
.container {
  display: flex;
  margin: 0 auto;
  justify-content: center;
  direction: rtl;
  width: 100%;
  transition: all 0.4s;
  z-index: 998;
}
.scroll {
  background-color: #373737;
  border-radius: 0px;
  margin-top: -150px;
  justify-content: center;
  display: flex;
  direction: rtl;
  transition: all 0.4s;
  padding: -5px 12px;
  height: 80px;
  width: 100%;
  position: fixed;
  z-index: 999;
  @media only screen and (max-width: 1100px) {
    display: none;
  }
}

a {
  color: white;
  font-size: 18px;
  text-decoration: none;
}

.badge {
  background-color: red;
  z-index: 999;
  position: absolute;
  border-radius: 50px;
  padding-top: 2px;
  padding-right: 7px;
  padding-left: 7px;
  top: 6rem;
}
.popup {
  height: 100vh;
  width: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.836);
  opacity: 1;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>