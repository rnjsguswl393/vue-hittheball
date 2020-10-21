<template>
	<div class="details">
		<div class="container">
			<div class="row">
				<!-- <markereve @markerevent="markerev"></markereve> -->
				<div class="col-md-12" v-for="(product,index) in products" :key="index"> 
					<div v-if="proId == product.productId">
						<h1>{{product.productTitle}}</h1>
						<img :src="product.image" class="img-fluid"> <br><br>
						<p>{{product.productText}}</p>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	export default{
		name:'details',
		data(){
			return{
				proId:this.$route.params.Pid,
				title:"details",
				products:[
				{
				productTitle:"@@연습장",
				image       : require('../assets/images/product1.png'),
				productId:1,
				productText:"아주좋고 아주 좋아요",
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
				{
				productTitle:"MLI",
				image       : require('../assets/images/product5.png'),
				productId:5
				},
				{
				productTitle:"Banans",
				image       : require('../assets/images/product6.png'),
				productId:6
				}
				]
				 
			}
		},
		
		
	}
</script>