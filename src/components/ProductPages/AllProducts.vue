<template>
  <div>
    <div class="total">
      <div class="left">
        <div class="u-center">
      <h2 class="header-title"> محصولات</h2>
    </div>
        <div class="row">
          <div class="box" v-for="product in FilterProducts" :key="product.id">
            <router-link
              :to="{ name: 'singleproduct', params: { slug: product.slug } }"
              class="product-info"
            >
              <transition name="bounce">
                <img
                  :class="{ blurimg: !product.available }"
                  class="image"
                 
                  :src="`https://sdriedf.ir` + product.picture[0].picture"
                  alt=""
                />
              </transition>
              <p class="paragraph">{{ product.name }}</p>
              <h5 class="cost">
                {{ product.show_cost.toLocaleString() }} تومان
              </h5>
              <router-link
                :to="{ name: 'singleproduct', params: { slug: product.slug } }"
                class="btn"
                >مشاهده محصول</router-link
              >
              <p class="available" v-if="!product.available">
                کالای مورد نظر موجود نیست!
              </p>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: true,
    };
  },
  metaInfo: {
    title: "فروشگاه اینترنتی",
    titleTemplate: "%s - محصولات",
    htmlAttrs: {
      lang: "utf-8",
      amp: true,
    },
  },
  computed: {
    FilterProducts() {
      return this.$store.getters.GetProducts;
    },
  },
  created() {
    this.$store.dispatch("GetProductsFromServer");
  },
};
</script>

<style lang="scss" scoped>
$color-primary-dark: orange;
$color-primary-light: orangered;
.u-center {
  margin: 10px 0 50px 0;
  text-align: center;
}
.header-title {
  font-size: 2rem;
  font-weight: 700;
  background-image: linear-gradient(to right, orange, orangered);
  -webkit-background-clip: text;
  display: inline-block;
  color: transparent;
  transition: all 0.3s;
}



.product-info {
  color: black;
  text-decoration: none;
}
.available {
  font-size: 12px;
  direction: rtl;
  color: red;
  background-color: white;
  padding: 2px;
  border-radius: 5px;
  position: absolute;
  font-weight: bold;
  margin-right: 50px;
  margin-top: -230px;
  animation: shake-animation 4.72s ease infinite;
  transform-origin: 50% 50%;
}

@keyframes shake-animation {
  0% {
    transform: translate(0, 0);
  }
  1.78571% {
    transform: translate(5px, 0);
  }
  3.57143% {
    transform: translate(0, 0);
  }
  5.35714% {
    transform: translate(5px, 0);
  }
  7.14286% {
    transform: translate(0, 0);
  }
  8.92857% {
    transform: translate(5px, 0);
  }
  10.71429% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(0, 0);
  }
}
.blurimg {
  filter: blur(4px);
}
.total {
  height: auto;
  display: flex;
  direction: rtl;
  padding: 2rem 0 5rem 0;
}
.left {
  width: 100%;
  margin: 0 auto;
}
.row {
  display: flex;
  flex-wrap: wrap;
  width: 90%;
  margin: 0 auto;
  justify-content: center;
}
.box {
  background-color: white;
  text-align: center;
  padding: 15px;
  margin-left: 20px;
  margin-top: 20px;
  width: 310px;
  transition: all 0.5s;
  box-shadow: 0.5rem 1rem 1rem rgba(rgb(163, 158, 158), 0.5);

  @media screen and (max-width: 740px) {
    width: 190px;
  }
  @media screen and (max-width: 500px) {
    width: 170px;
  }
  @media screen and (max-width: 447px) {
    width: 100%;
  }
}
.image {
  width: 100%;
  height: 190px;
  transition: all 1s;
  &:hover {
    transform: scale(1.1);
    opacity: 1s;
  }
}

.blurimg {
  filter: blur(7px);
}
.paragraph {
  display: block;
  font-weight: bold;
  font-size: 22px;
  margin-bottom: 10px;
}

.btn {
  display: block;
  text-align: center;
  background-color: orange;
  text-decoration: none;
  padding: 10px 5px;
  font-size: 16px;
  margin-top: 10px;
  cursor: pointer;
  color: white;
  border: 1px solid orange;
  transition: all 0.5s;
  @media screen and (max-width: 500px) {
  }
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>