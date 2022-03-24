<template>
  <div class="nav-container">
    <div class="nav-bar">
      <h2>Product Cockpit</h2>
    </div>
  </div>
  <div class="search-container">
    <form class="search-bar">
    <input type="text" name="ProductNumber" id="ProductNumber" placeholder="Suche nach Artikelnummer..." ref="productNumberInput">
    <button @click.prevent="getInput()" class="searchButton"></button>
    </form>
  </div>
  
  <div class="product-container" v-if="showProduct">
     <div class="product-image">
        <img v-bind:src="image" alt="Produktfoto">    
    </div>
    <div class="product-information">
      <h3>Produktinformationen:</h3>
      <br>
      <br>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Expedita, placeat dolorem inventore exercitationem fugiat asperiores repudiandae id, reprehenderit esse cupiditate quaerat enim fuga eaque. Asperiores, porro iste voluptas laboriosam consectetur in sapiente et odit repudiandae optio ea ducimus a cum blanditiis voluptatibus perferendis voluptatem harum labore praesentium deleniti ab quam.</p>
    </div>
    <div class="product-rating">
      <h3>Bewertung:</h3>
        <tbody>
          <tr
          v-for="rating in ratings"
          :key="rating.name"
          >
            <td>{{rating.name}}</td>
            <td>{{rating.value}}</td>
          </tr>
        </tbody>
    </div>
  </div>
  <div class="substitute-container" v-if="showProduct">
    <div class="substitute-header">
      <h2>Alternativen</h2>
      <!-- <button id="substitute-arrow"></button> -->
    </div>
  </div>
</template>
<script>
const ratings = [
  {
    name: 'Qualität',
    value: 'E'
  },
  {
    name: 'Nutzen',
    value: 5
  },
  {
    name: 'Teilsortiment',
    value: 'Aluminium'
  }
]
let showProduct = false;
export default {
  created(){
    // this.getApiResponse()
  },
  data(){
    return{
      productNumberInput:"",
      ratings,
      showProduct,
      deepLink:undefined,
      product:undefined,
      image:"",
    }
  },

  methods:{
     async getInput(){
      this.productNumberInput = this.$refs.productNumberInput.value;
      console.log(this.productNumberInput)
      
      const myHeaders = new Headers();
      myHeaders.append("x-requested-by", "<-->666<-->");

      const requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow',
      };
      
      const deep = await fetch(`http://mybackend.com:8080/api/shops/www.kaiserkraft.de/search_suggest?query=${this.productNumberInput}&requestId=607f91cc-55e9-43ab-8cc6-bf42287892ef&channelId=kkeu_de_DE`, requestOptions)
        .then(response => response.json())
        .catch(error => console.log('error', error));
      let deepLink = deep.productsSuggestions;
      deepLink = JSON.stringify(deepLink[0].deeplink).substring(deepLink[0].deeplink.indexOf('/M') +1);
      const deepLinkLength = deepLink.length;
      this.deepLink = deepLink.substring(1, deepLinkLength -2)
      console.log(this.deepLink);

      const product = await fetch(`http://mybackend.com:8080/api/shops/www.kaiserkraft.de/product/${this.deepLink}?lang=de`, requestOptions)
        .then((response) => response.json())
        .catch((error) => {
          console.log('error', error);
          this.showProduct = false;
          });
      this.product = product;
      console.log(this.product);
      
      
      if(this.product != undefined){
        this.image = this.product.galleryList[0].url;  
        this.showProduct = true;
      }
    }
  }
}
</script>
