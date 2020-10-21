<template>
  <div class="home">
    <br>
    <p>{{title}}</p>
    <div class="row" style="margin:0 auto; " >
      <router-link class="card" to="/services"   >
        <div style="float:left">
          <img src="../assets/images/golf1.png" class="card-img-top"  >
        </div>
      </router-link>
      <router-link class="card" to="/blog">
          <div style="float:right">
            <img src="../assets/images/golf2.png" class="card-img-top"  >
          </div>
      </router-link>
        <!-- <div class="card" style="width: 10rem; margin:auto" v-for="(data,index) in products" :key="index">
          <img :src="data.image" class="card-img-top" @click="goTodetail(data.productId)">

        </div> -->
   
    </div>
     <br>
    <p>{{subtitle}}</p>
    <div class="next" style="margin:0 auto">
      <div class="card" style="width: 10rem; float:left" v-for="(data,index) in products" :key="index">
          <img :src="data.image" class="card-img-top" @click="goTodetail(data.productId)">
          <div class="card-body">
            <h5 class="card-title">{{data.productTitle}}</h5>
            <p class="card-text">
              현재위치에서 약 {{data.productId}}km
            </p>
          </div>
        </div>
      <!-- <router-link class="nav-link" to="/blog"> -->
      
    </div>
    
  </div>

</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      title: '가까운 장소로 알아보고 있나요?',
      subtitle:'정확한 자세와 스윙연습이 필요한가요?',
      // products:ttt.data.products
      products:
      [
      {
        productTitle:"@@연습장",
        image       : require('../assets/images/product1.png'),
        productId:1
      },
      {
        productTitle:"##연습장",
        image       : require('../assets/images/product2.png'),
        productId:2
      },
      {
        productTitle:"$$연습장",
        image       : require('../assets/images/product3.png'),
        productId:3
      },
      {
        productTitle:"&&연습장",
        image       : require('../assets/images/product4.png'),
        productId:4
      },
      // {
      //   productTitle:"MLI",
      //   image       : require('../assets/images/product5.png'),
      //   productId:5
      // },
      // {
      //   productTitle:"Banans",
      //   image       : require('../assets/images/product6.png'),
      //   productId:6
      // }
      ]
    }
  },
  methods:{
  goTodetail(prodId) {
    let proId=prodId
    this.$router.push({name:'details',params:{Pid:proId}})
  }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.row img{
  /* max-height: 15em; */
  width: 100%;
  max-width: 11em;
  height: 13em;

}
.next img{
  /* max-height: 15em; */
  width: 100%;
  height: 15em;

}
/* .row h3{
  cursor:pointer;
} */
</style>
