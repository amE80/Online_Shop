<template>
  <div>
    <div class="shop-box">
      <div class="right">
        <div class="product-info">
          <h1 class="product-name">{{ singleProduct.name }}</h1>
          <div class="cost-box" v-if="singleProduct.product_cost">
            <p v-if="Select == 'انتخاب حجم' && jar" class="alert">
              برای نمایش قیمت ، ابتدا حجم شیشه را مشخص کنید
            </p>
            <p v-if="Select == 'انتخاب حجم' && envelope" class="alert">
              برای نمایش قیمت ، ابتدا حجم پاکت را مشخص کنید
            </p>

            <h2 v-if="Select !== 'انتخاب حجم'" style="direction: rtl">
              {{ singleProduct.product_cost[Select].cost.toLocaleString() }}
              تومان
            </h2>
          </div>
          <div style="direction: rtl" class="count-box">
            <h2 class="count-lable">تعداد:</h2>
            <input
              v-model="count"
              class="count"
              type="number"
              min="1"
              max="10"
              value="1"
            />
          </div>
          <div class="pack">
            <h3 style="direction: rtl">نوع دسته بندی :</h3>
            <div
              @click="activeJar"
              class="combine"
              style="display: inline-block"
            >
              <img
                class="pack-icon pack-jar"
                src="../../../public/img/jar.png"
                alt=""
              />
              <font-awesome-icon
                v-if="jar"
                class="check-icon check-jar"
                icon="check"
              />
            </div>
            <div
              @click="activeEnvelope"
              class="combine"
              style="display: inline-block"
            >
              <img
                class="pack-icon pack-envelope"
                src="../../../public/img/envelope.png"
                alt=""
              />
              <font-awesome-icon
                v-if="envelope"
                class="check-icon check-envelope"
                icon="check"
              />
            </div>
          </div>

          <div v-if="jar" class="weight">
            <select v-model="Select" class="select">
              <option value="انتخاب حجم">بسته بندی شیشه ای</option>
              <option
                :value="index"
                v-for="(pack, index) in singleProduct.product_cost"
                :key="pack.id"
                v-show="pack.pack.parent == 5"
              >
                {{ pack.pack.weight }} گرم
              </option>
            </select>
          </div>

          <div v-if="envelope" class="weight">
            <select v-model="Select" class="select">
              <option disabled value="انتخاب حجم">بسته بندی پاکتی</option>
              <option
                :value="index"
                v-for="(pack, index) in singleProduct.product_cost"
                :key="pack.id"
                v-show="pack.pack.parent == 1"
              >
                {{ pack.pack.weight }} گرم
              </option>
            </select>
          </div>
          <br />
          <div style="direction: rtl" class="addtobasket">
            <a
              type="submit"
              class="basket"
              :class="{ disabled: Select == 'انتخاب حجم' }"
              @click="
                AddToOrder(
                  singleProduct.id,
                  singleProduct.product_cost[Select].id
                )
              "
              >افزودن به سبد خرید <font-awesome-icon icon="shopping-cart"
            /></a>
          </div>
        </div>
      </div>
      <div class="left">
            <img v-if="singleProduct.picture" class="img-carousel" :src="`https://sdriedf.ir`+ singleProduct.picture[0].picture" alt="" />
      </div>
    </div>
    <div class="product-explanation" v-html="singleProduct.description"></div>

    <similar-product></similar-product>
    <div class="product-properties"></div>
    <the-questions></the-questions>
  </div>
</template>


<script>
import Vue from "vue";
import SimilarProduct from "./SimilarProduct.vue";
import TheQuestions from "../Questions/TheQuestions.vue";
export default {
  components: {
    // Carousel,
    // Slide,
    SimilarProduct,
    TheQuestions,
  },
  data() {
    return {
      cost: 50000,
      jar: false,
      envelope: true,
      Select: 0,
      count: 1,
      title: this.$route.params.slug,
    };
  },
  metaInfo() {
    return {
      titleTemplate: `فروشگاه اینترنتی - ${this.title}`,
      htmlAttrs: {
        lang: "utf-8",
        amp: true,
      },
    };
  },
  methods: {
    activeJar() {
      (this.jar = true), (this.envelope = false), (this.Select = "انتخاب حجم");
    },
    activeEnvelope() {
      (this.envelope = true), (this.jar = false), (this.Select = 0);
    },
    AddToOrder(resId, packID) {
      const orderDetail = {
        product: resId,
        amount: this.count,
        pack: packID,
      };
      console.log(orderDetail);
      if (orderDetail.amount < 1) {
        Vue.swal("توجه", "تعداد کالا را درست انتخاب کنید", "info");
      } else {
        this.$store.dispatch("AddProductToOrder", orderDetail);
      }
    },
  },
  computed: {
    singleProduct() {
      return this.$store.getters.GetSingleProduct;
    },
    SimilarProduct() {
      return this.$store.getters.GetSimilarProduct;
    },
  },
  created() {
    this.$store.dispatch("GetSinlgeProductsFromServer", {
      slug: this.$route.params.slug,
    });
    this.$store.dispatch("GetSimilarProductsFromServer", {
      slug: this.$route.params.slug,
    });
  },
  watch: {
    $route() {
      this.$store.dispatch("GetSinlgeProductsFromServer", {
        slug: this.$route.params.slug,
      });
    },
  },
};
</script>


<style lang="scss" scoped>
.alert {
  color: red;
}
.shop-box {
  width: 80%;
  height: 500px;
  margin: 6rem auto;
  display: flex;
  flex-wrap: wrap-reverse;
  justify-content: center;
  @media screen and (max-width: 1065px) {
    width: 90%;
  }
  @media screen and (max-width: 950px) {
    width: 95%;
  }
  @media screen and (max-width: 686px) {
    margin-top: 25rem;
  }
  a.disabled {
    // pointer-events: none;
    cursor: not-allowed;
  }
}
.left {
  // border: 1px solid black;
  // padding: 10px;
box-shadow: rgba(0, 0, 0, 0.25) 0px 25px 50px -12px;  width: 400px;
  height: 100%;
  @media screen and (max-width: 895px) {
    width: 300px;
  }
  @media screen and (max-width: 686px) {
    width: 85%;
    height: 300px;
    margin-top: 25rem;
  }
}

.right {
  background-color: whitesmoke;
  border: 1px solid black;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 20px 25px -5px,
    rgba(0, 0, 0, 0.04) 0px 10px 10px -5px;
  width: 450px;
  height: 500px;
  padding-right: 15px;
  @media screen and (max-width: 895px) {
    width: 350px;
    padding-right: 0px;
  }
  @media screen and (max-width: 686px) {
    width: 85%;
    height: 520px;
  }
  @media screen and (max-width: 450px) {
    height: 550px;
  }
}
.product-name {
  margin-top: 15px;
  margin-bottom: 0.4rem;

  @media screen and (max-width: 450px) {
    font-size: 22px;
    margin: 2rem 0 1rem 0;
  }
}
.count-lable {
  @media screen and (max-width: 450px) {
    font-size: 18px;
  }
}
.carousel {
  height: 100%;
}
.img-carousel {
  width: 100%;
  height: 500px;
  @media screen and (max-width: 686px) {
    height: 300px;
  }
}
.product-info {
  text-align: center;
}
.cost-box {
  background-color: white;
  border-radius: 5px;
  width: 250px;
  display: inline-block;
  padding: 10px;
  border: 1px solid black;
  @media screen and (max-width: 385px) {
    width: 200px;
  }
  @media screen and (max-width: 450px) {
    margin: 15px 0;
  }
}
input {
  text-indent: 10px;
}
option {
  font-family: "BYekan";
}
.count {
  background-color: white;
  display: inline-block;
  padding: 10px;
  width: 150px;
  border-radius: 10px;
  font-size: 20px;

  @media screen and (max-width: 450px) {
    margin: 0.5rem 0;
  }
}
.product-explanation {
  width: 80%;
  margin: -50px auto 3rem auto;
  direction: rtl;
}
.select {
  padding: 10px;
  font-family: "BYekan";
  direction: rtl;
  border-radius: 10px;
  cursor: pointer;
  margin: 1rem 0 1rem 0;
}

.pack-icon {
  width: 70px;
}
.pack {
  margin-top: 10px;
}
.basket {
  background-color: orange;
  color: white;
  cursor: pointer;
  padding: 10px;
  border-radius: 10px;
  transition: all 0.4s;
}
.basket:hover {
  background-color: orangered;
}
.check-icon {
  font-size: 30px;
  color: green;
  position: absolute;
}
.check-envelope {
  margin-left: -48px;
  margin-top: 20px;
}
.check-jar {
  margin-left: -48px;
  margin-top: 20px;
}
.combine {
  cursor: pointer;
}
</style>