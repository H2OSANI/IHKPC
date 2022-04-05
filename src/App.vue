<template>
  <div class="nav-container">
    <div class="nav-bar">
      <h2>Product Cockpit</h2>
    </div>
  </div>
  <div class="search-container">
    <div class="price" v-if="showProduct">
      <h3>Preis: {{ price }} €</h3>
    </div>
    <form class="search-bar">
      <input
        type="text"
        name="ProductNumber"
        id="ProductNumber"
        placeholder="Suche nach Artikelnummer..."
        ref="productNumberInput"
      />
      <button @click.prevent="getInput()" class="searchButton"></button>
    </form>
  </div>

  <div class="product-container" v-if="showProduct">
    <div class="product-image">
      <img v-bind:src="image" alt="Produktfoto" />
    </div>
    <div class="product-information">
      <h3>Produktinformationen:</h3>
      <br />
      <ul>
        <li>{{ description }}</li>
        <li v-for="feature in features" :key="feature.name">
          {{ feature.name }} : {{ feature.featureValues[0].value }}
        </li>
      </ul>
    </div>
    <div class="product-rating">
      <h3>Bewertung:</h3>
      <tbody>
        <tr>
          <td>Qualität:</td>
          <td>{{ ratings.Quality }}</td>
        </tr>
        <tr>
          <td>Nutzen:</td>
          <td>{{ ratings.Nutzen }}</td>
        </tr>
        <tr>
          <td>Teilsortiment:</td>
          <td>{{ ratings.Teilsortiment }}</td>
        </tr>
      </tbody>
    </div>
  </div>
  <div class="substitute-container" v-if="showProduct">
    <div class="substitute-header">
      <h2>Alternativen</h2>
    </div>
    <svg
      @click="toggleAlternatives()"
      v-if="!showAlternatives"
      class="alternative-button"
      xmlns="http://www.w3.org/2000/svg"
      width="80"
      height="80"
      xml:space="preserve"
    >
      <a>
        <rect width="100%" height="100%" fill="transparent" />
        <path
          vector-effect="non-scaling-stroke"
          d="M54.939 38.815c1.294 1.315 1.294 3.447 0 4.74l-12.561 12.56a3.338 3.338 0 0 1-4.74 0l-12.561-12.56c-1.316-1.293-1.316-3.425 0-4.74 1.814-1.7 6.802 2.041 11.292 3.787V25.12c0-1.225.997-2.222 2.222-2.222h3.355c1.225 0 2.222.997 2.222 2.222v17.255c4.24-1.791 8.776-5.102 10.771-3.56z"
          fill="#ffe200"
        />
        <path
          vector-effect="non-scaling-stroke"
          d="M40 62.724C27.47 62.724 17.276 52.53 17.276 40S27.47 17.276 40 17.276 62.724 27.47 62.724 40 52.53 62.724 40 62.724zm0-44.388c-11.946 0-21.664 9.718-21.664 21.664 0 11.945 9.718 21.664 21.664 21.664 11.945 0 21.664-9.719 21.664-21.664 0-11.946-9.719-21.664-21.664-21.664z"
          fill="#ffe200"
        />
      </a>
    </svg>
    <svg
      @click="toggleAlternatives()"
      v-if="showAlternatives"
      class="alternative-button-after"
      xmlns="http://www.w3.org/2000/svg"
      width="80"
      height="80"
      xml:space="preserve"
    >
      <a>
        <rect width="100%" height="100%" fill="transparent" />
        <path
          vector-effect="non-scaling-stroke"
          d="M54.939 38.815c1.294 1.315 1.294 3.447 0 4.74l-12.561 12.56a3.338 3.338 0 0 1-4.74 0l-12.561-12.56c-1.316-1.293-1.316-3.425 0-4.74 1.814-1.7 6.802 2.041 11.292 3.787V25.12c0-1.225.997-2.222 2.222-2.222h3.355c1.225 0 2.222.997 2.222 2.222v17.255c4.24-1.791 8.776-5.102 10.771-3.56z"
          fill="#ffe200"
        />
        <path
          vector-effect="non-scaling-stroke"
          d="M40 62.724C27.47 62.724 17.276 52.53 17.276 40S27.47 17.276 40 17.276 62.724 27.47 62.724 40 52.53 62.724 40 62.724zm0-44.388c-11.946 0-21.664 9.718-21.664 21.664 0 11.945 9.718 21.664 21.664 21.664 11.945 0 21.664-9.719 21.664-21.664 0-11.946-9.719-21.664-21.664-21.664z"
          fill="#ffe200"
        />
      </a>
    </svg>
  </div>
  <div class="alternative-container" v-if="showAlternatives">
    <table id="alternative-table">
      <thead>
        <tr>
          <th>&nbsp;</th>
          <th>A</th>
          <th>B</th>
          <th>C</th>
          <th>D</th>
          <th>E</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="alternative-row">5</td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">4</td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">3</td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">2</td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">1</td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
          <td>
            <div class="alternative-product">
              <img v-bind:src="image" alt="Produktfoto" />
              <p>{{ description }}</p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import jsonData from "@/json/data.json";
let showProduct = false;
export default {
  created() {},
  data() {
    return {
      productNumberInput: "",
      ratings: [
        {
          Quality: "",
          Nutzen: 0,
          Teilsortiment: "",
        },
      ],
      showProduct,
      deepLink: undefined,
      product: undefined,
      image: "",
      price: null,
      description: "",
      features: [],
      showAlternatives: false,
      data: jsonData,
      options: [],
    };
  },

  methods: {
    async getInput() {
      this.options = [];
      this.productNumberInput = this.$refs.productNumberInput.value;
      console.log(this.productNumberInput);

      const myHeaders = new Headers();
      myHeaders.append("x-requested-by", "<-->666<-->");

      const requestOptions = {
        method: "GET",
        headers: myHeaders,
        redirect: "follow",
      };
      // Test search service api
      // const test = await fetch(
      //   "http://mybackend.com:8080/search/productCockpitSearch?channelId=kkeu_de_DE&customerId=www.kaiserkraft.de-userpricegroup&productsPerPage=10&query=60-l-Auffangwannen"
      // )
      //   .then((response) => response.json())
      //   .catch((error) => console.log("error", error));
      // console.log(test);

      const data = await fetch(
        //search = ID Search
        //search_suggest = Vorschläge
        //productCockpitSearch = return ID Alternativen
        `http://mybackend.com:8080/shop/api/shops/www.kaiserkraft.de/search?query=${this.productNumberInput}&productsPerPage=24&page=1&customerId=www.kaiserkraft.de-userpricegroup&sessionId=06a9554d-8224-413d-a2ea-98a5659284eb&channelId=kkeu_de_DE&sort=`,
        requestOptions
      )
        .then((response) => response.json())
        .catch((error) => console.log("error", error));
      console.log(data);
      const productMaster = data.records[0].MasterID;
      const articleNumber = data.records[0].ArticleNumber;
      console.log(productMaster);
      console.log(articleNumber);
      const productDetails = await fetch(
        `http://mybackend.com:8080/shop/api/shops/www.kaiserkraft.de/product/${productMaster}/?articleNumber=${articleNumber}?lang=de`,
        requestOptions
      )
        .then((response) => response.json())
        .catch((error) => {
          console.log("error", error);
          this.showProduct = false;
        });
      this.product = productDetails;
      console.log(this.product);

      if (this.product != undefined) {
        this.image = this.product.galleryList[0].url;
        this.price = this.product.price.net.toFixed(2);
        this.description = this.product.brandedProductName;
        const classifications = this.product.classifications;
        this.features = classifications[0].features;
        console.log(this.features);
        this.showProduct = true;
        this.showAlternatives = false;
      }
      const result = this.data.Sheet1.find((item) => item.ID === articleNumber);
      console.log(result);

      this.ratings.Quality = result.Quality;
      this.ratings.Nutzen = result.Nutzen;
      this.ratings.Teilsortiment = result.Teilsortiment;

      for (let i = 0; i < this.data.Sheet1.length; i++) {
        let obj = this.data.Sheet1[i];

        if (obj.Type === result.Type) {
          this.options.push(obj);
        }
      }
      console.log(this.options);
      console.log(
        this.options.find((item) => item.Quality === "E" && item.Nutzen == 1)
      );
    },
    toggleAlternatives() {
      this.showAlternatives = !this.showAlternatives;
    },
  },
};
</script>
