<template>
  <div>
    <section class="section-features">
      <img class="back-img" :src=" `https://sdriedf.ir` +GetInformation[0].site_img" alt="">
      <div class="shadow"></div>
       <div class="row">
        <div class="col-1-of-4">
          <div class="feature-box">
            <font-awesome-icon class="fa" icon="check-circle" />
            <h3 class="heading-tertiary">ضمانت و تضمین اصل بودن</h3>
            <p class="feature-box__text">لذت خرید کالای درجه یک و اصل</p>
          </div>
        </div>
        <div class="col-1-of-4">
          <div class="feature-box">
            <font-awesome-icon class="fa" icon="shopping-bag" />
            <h3 class="heading-tertiary">خرید راحت</h3>
            <p class="feature-box__text">
             به راحتی خرید کنید
            </p>
          </div>
        </div>
        <div class="col-1-of-4">
          <div class="feature-box">
            <font-awesome-icon class="fa" icon="credit-card" />
            <h3 class="heading-tertiary">پرداخت آنی و سفارش آنلاین</h3>
            <p class="feature-box__text">پرداخت بدون دردسر و امن در خانه</p>
          </div>
        </div>
        <div class="col-1-of-4">
          <div class="feature-box">
            <font-awesome-icon class="fa" icon="truck" />
            <h3 class="heading-tertiary">تحویل سریع</h3>
            <p class="feature-box__text">ارسال کالا به تمام نقاط ایران</p>
          </div>
        </div>
      </div>
    </section>
    <mostsell-product></mostsell-product>
    <best-product></best-product>
  </div>
</template>

<script>
import BestProduct from './BestProduct.vue';
import MostsellProduct from "./MostsellProduct.vue";
export default {
  components: { MostsellProduct, BestProduct },
  computed:{
     GetInformation() {
      return this.$store.getters.GetSiteInformation;
    },
  }
};
</script>


<style lang="scss" scoped>
.heading-tertiary {
  font-size: 15px;
}
.row {
  display: flex;
  align-items: flex-end;
  justify-content: space-evenly;
  flex-wrap: wrap;
}
.shadow{
  background-color: rgba(0, 0, 0, 0.658);
  width: 100%;
  height: 500px;
  position: absolute;
  z-index: -10;
    width: 100%;
    margin-top: -10rem;
    transform: skewY(-5deg);

  & > * {
    transform: skewY(5deg);
  }
   @media screen and (max-width: 450px) {
    height: 550px;
      }
        @media screen and (max-width: 401px) {
    height: 950px;
      }
}
.feature-box {
  background-color: rgba(255, 255, 255, 0.37);
  width: 250px;
  font-size: 20px;
  padding: 2.5rem 0px;
  text-align: center;
  border-radius: 5px;
  transition: all 0.3s;
  color: white;
  z-index: 999;
  margin-top: -50px;
  margin-bottom: 65px;

  @media screen and (max-width: 500px) {
    width: 170px;
      }
        @media screen and (max-width: 450px) {
    width: 200px;
      }

  &__icon {
    font-size: 50px;
    margin-bottom: 20px;
    @media screen and (max-width: 450px) {
    font-size: 40px;
    margin-bottom: 25px;
      }
  }
  &__text {
    font-size: 12px;
    margin-top: 5px;
     @media screen and (max-width: 450px) {
    font-size: 15px;
    margin-top: 20px;
      }
  }
}
.back-img{
  width: 100%;
  height: 500px;
  z-index: -9999;
  // margin-bottom: 1rem;
  position: absolute;
  margin-top: -10rem;
    transform: skewY(-5deg);

  & > * {
    transform: skewY(5deg);
  }
   @media screen and (max-width: 450px) {
    height: 550px;
      }
  // @media screen and (max-width: 500px) {
  //   height: 550px;
  //   margin-bottom: 2rem;
  //     }
      @media screen and (max-width: 401px) {
    height: 950px;
      }
}
.section-features {
  margin-top: 50px;
}

.fa {
  font-size: 35px;
}
</style>
