<template>
  <div class="nav-container">
    <div class="nav-bar">
      <h2>Product Cockpit</h2>
    </div>
  </div>
  <div class="search-container">
    <form action="submit" class="search-bar">
    <input type="text" name="ProductNumber" id="ProductNumber" placeholder="Suche nach Artikelnummer...">
    <button type="submit"></button>
    </form>
  </div>
  
  <div class="product-container">
     <div class="product-image">
        <img src="https://images.kkeu.de/is/image/BEG/Leitern_und_Ger%C3%BCste/Stehleitern/Stufen-Stehleiter_einseitig_pdplarge-mrd--945299_AFS_00_00_00_9897134.jpg" alt="Produktfoto">    
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
  <div class="substitute-container">
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
export default {
  created(){
    this.getApiResponse()
  },

  methods:{
    getApiResponse() {
      const myHeaders = new Headers();
      myHeaders.append("x-requested-by", "<-->666<-->");

      const requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow',
      };

      fetch("http://mybackend.com:8080/api/shops/www.kaiserkraft.de/product/M6615896?lang=de", requestOptions)
        .then(response => response.json())
        .then(result => console.log(result))
        .catch(error => console.log('error', error));
        
    }  
  },

  data() {
    return {
    ratings
    }
  }
}
</script>
