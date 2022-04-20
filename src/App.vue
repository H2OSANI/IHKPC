<template>
  <div class="nav-container">
    <div class="nav-bar">
      <h2>Product Cockpit</h2>
    </div>
    <div class="countrySelection">
        <select v-model="countrySelected">
          <option v-for="c in country" :key="c.Name" v-bind:value="{Name: c.Name, Code: c.Code, Currency: c.Currency}" >{{ c.Name }}</option>
        </select>
    </div>
  </div>
  <div class="search-container">
    <div class="price" v-if="showProduct">
      <h3>Preis: {{ price }} {{ countrySelected.Currency }}</h3>
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
    <div>
    <svg
      class="alternativebutton"
      :class="{ alternativebuttondisabled: !loadAlternatives, alternativebuttonafter: showAlternatives}"
      @click="toggleAlternatives()"
      v-bind="loadAlternatives"
      xmlns="http://www.w3.org/2000/svg"
      width="80"
      height="80"
      xml:space="preserve"
    >
      <a>
        <rect width="100%" height="100%" fill="transparent" />
        <path
          :class="{ arrowLoading: !loadAlternatives }"
          vector-effect="non-scaling-stroke"
          d="M54.939 38.815c1.294 1.315 1.294 3.447 0 4.74l-12.561 12.56a3.338 3.338 0 0 1-4.74 0l-12.561-12.56c-1.316-1.293-1.316-3.425 0-4.74 1.814-1.7 6.802 2.041 11.292 3.787V25.12c0-1.225.997-2.222 2.222-2.222h3.355c1.225 0 2.222.997 2.222 2.222v17.255c4.24-1.791 8.776-5.102 10.771-3.56z"
          fill="#ffe200"
        />
        <path
          :class="{ loading: !loadAlternatives }"
          d="M40 62.724C27.47 62.724 17.276 52.53 17.276 40S27.47 17.276 40 17.276 62.724 27.47 62.724 40 52.53 62.724 40 62.724zm0-44.388c-11.946 0-21.664 9.718-21.664 21.664 0 11.945 9.718 21.664 21.664 21.664 11.945 0 21.664-9.719 21.664-21.664 0-11.946-9.719-21.664-21.664-21.664z"
          fill="#ffe200"
        />
      </a>
    </svg>
    </div>
  </div>
  <transition>
  <div class="alternative-container" v-if="showAlternatives">
    <table id="alternative-table">
      <thead>
        <tr class="tr">
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
          <td v-for="alternative5 in five" :key="alternative5.brandedProductName" @click="alternative5.galleryList[0].url != null ? getInput(alternative5.articleNumber) : null">
            <div class="wrapper" :class="{alternativePointer : !alternative5.galleryList[0].url}">
              <div class="container">
                <div class="top">
                   <img v-if="alternative5.galleryList[0].url === null" src="@/assets/no-product.png" alt="no Match">
                    <img v-else v-bind:src="alternative5.galleryList[0].url">
                </div>
                <div class="bottom">
                  <div class="left">
                    <div class="details">
                      <h1>{{ alternative5.brandedProductName }}</h1>
                    </div>
                    <div class="buy"><h1>{{ alternative5.price.net.toFixed(2) }} {{ countrySelected.Currency }}</h1></div>
                  </div>
                  <div class="right">
                  </div>
                </div>
              </div>
              <div v-if="alternative5.galleryList[0].url != null" class="inside">
                <div class="icon"><i class="material-icons-outlined">info</i></div>
                <div class="contents">
                  <ul v-for="classification in alternative5.classifications" v-bind:value="classification.id" v-bind:selected="index === 0" :key="classification.id">
                    <li v-for="feature in classification.features" :key="feature.name">
                      {{ feature.name }} : {{ feature.featureValues[0].value }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">4</td>
           <td v-for="alternative4 in four" :key="alternative4.brandedProductName" @click="alternative4.galleryList[0].url != null ? getInput(alternative4.articleNumber) : null">
            <div class="wrapper" :class="{alternativePointer : !alternative4.galleryList[0].url}">
              <div class="container">
                <div class="top">
                  <img v-if="alternative4.galleryList[0].url === null" src="@/assets/no-product.png" alt="no Match">
                  <img v-else v-bind:src="alternative4.galleryList[0].url">
                </div>
                <div class="bottom">
                  <div class="left">
                    <div class="details">
                      <h1>{{ alternative4.brandedProductName }}</h1>
                    </div>
                    <div class="buy"><h1>{{ alternative4.price.net.toFixed(2) }} {{ countrySelected.Currency }}</h1></div>
                  </div>
                  <div class="right">
                  </div>
                </div>
              </div>
              <div v-if="alternative4.galleryList[0].url != null" class="inside">
                <div class="icon"><i class="material-icons-outlined">info</i></div>
                <div class="contents">
                  <ul v-for="classification in alternative4.classifications" v-bind:value="classification.id" v-bind:selected="index === 0" :key="classification.id">
                    <li v-for="feature in classification.features" :key="feature.name">
                      {{ feature.name }} : {{ feature.featureValues[0].value }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">3</td>
         <td v-for="alternative3 in three" :key="alternative3.brandedProductName" @click="alternative3.galleryList[0].url != null ? getInput(alternative3.articleNumber) : null">
            <div class="wrapper" :class="{alternativePointer : !alternative3.galleryList[0].url}">
              <div class="container">
                <div class="top">
                  <img v-if="alternative3.galleryList[0].url === null" src="@/assets/no-product.png" alt="no Match">
                  <img v-else v-bind:src="alternative3.galleryList[0].url">
                </div>
                <div class="bottom">
                  <div class="left">
                    <div class="details">
                      <h1>{{ alternative3.brandedProductName }}</h1>
                    </div>
                    <div class="buy"><h1>{{ alternative3.price.net.toFixed(2) }} {{ countrySelected.Currency }}</h1></div>
                  </div>
                  <div class="right">
                  </div>
                </div>
              </div>
              <div v-if="alternative3.galleryList[0].url != null" class="inside">
                <div class="icon"><i class="material-icons-outlined">info</i></div>
                <div class="contents">
                  <ul v-for="classification in alternative3.classifications" v-bind:value="classification.id" v-bind:selected="index === 0" :key="classification.id">
                    <li v-for="feature in classification.features" :key="feature.name">
                      {{ feature.name }} : {{ feature.featureValues[0].value }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">2</td>
          <td v-for="alternative2 in two" :key="alternative2.brandedProductName" @click="alternative2.galleryList[0].url != null ? getInput(alternative2.articleNumber) : null">
            <div class="wrapper" :class="{alternativePointer : !alternative2.galleryList[0].url}">
              <div class="container">
                <div class="top">
                  <img v-if="alternative2.galleryList[0].url === null" src="@/assets/no-product.png" alt="no Match">
                  <img v-else v-bind:src="alternative2.galleryList[0].url">
                </div>
                <div class="bottom">
                  <div class="left">
                    <div class="details">
                      <h1>{{ alternative2.brandedProductName }}</h1>
                    </div>
                    <div class="buy"><h1>{{ alternative2.price.net.toFixed(2) }} {{ countrySelected.Currency }}</h1></div>
                  </div>
                  <div class="right">
                  </div>
                </div>
              </div>
              <div v-if="alternative2.galleryList[0].url != null" class="inside">
                <div class="icon"><i class="material-icons-outlined">info</i></div>
                <div class="contents">
                  <ul v-for="classification in alternative2.classifications" v-bind:value="classification.id" v-bind:selected="index === 0" :key="classification.id">
                    <li v-for="feature in classification.features" :key="feature.name">
                      {{ feature.name }} : {{ feature.featureValues[0].value }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <td class="alternative-row">1</td>
          <td v-for="alternative1 in one" :key="alternative1.brandedProductName" @click="alternative1.galleryList[0].url != null ? getInput(alternative1.articleNumber) : null">
            <div class="wrapper" :class="{alternativePointer : !alternative1.galleryList[0].url}">
              <div class="container">
                <div class="top">
                  <img v-if="alternative1.galleryList[0].url === null" src="@/assets/no-product.png" alt="no Match">
                  <img v-else v-bind:src="alternative1.galleryList[0].url">
                </div>
                <div class="bottom">
                  <div class="left">
                    <div class="details">
                      <h1>{{ alternative1.brandedProductName }}</h1>
                    </div>
                    <div class="buy"><h1>{{ alternative1.price.net.toFixed(2) }} {{ countrySelected.Currency }}</h1></div>
                  </div>
                  <div class="right">
                  </div>
                </div>
              </div>
              <div v-if="alternative1.galleryList[0].url != null" class="inside">
                <div class="icon"><i class="material-icons-outlined">info</i></div>
                <div class="contents">
                  <ul v-for="classification in alternative1.classifications" v-bind:value="classification.id" v-bind:selected="index === 0" :key="classification.id">
                    <li v-for="feature in classification.features" :key="feature.name">
                      {{ feature.name }} : {{ feature.featureValues[0].value }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  </transition>
</template>
<script>
const STORAGE_KEY = 'component-storage';
import jsonData from "@/json/data.json";
export default {
  data() {
    return {
      country: [
      {
        Name: 'Deutschland',
        Code: 'de',
        Currency: '€'
      },
      {
        Name: 'UK',
        Code: 'co.uk',
        Currency: '£'
      },
      {
        Name: 'Schweiz',
        Code: 'ch',
        Currency: 'CHF'
      },
      {
        Name: 'Austria',
        Code: 'at',
        Currency: '€'
      },
      {
        Name: 'Frankreich',
        Code: 'fr',
        Currency: '€'
      },
      {
        Name: 'Spanien',
        Code: 'es',
        Currency: '€'
      },
       {
        Name: 'Niederlande',
        Code: 'nl',
        Currency: '€'
      },
       {
        Name: 'Belgien',
        Code: 'be',
        Currency: '€'
      },
       {
        Name: 'Italien',
        Code: 'it',
        Currency: '€'
      },
       {
        Name: 'Tschechien',
        Code: 'cz',
        Currency: 'Kč'
      },
       {
        Name: 'Polen',
        Code: 'pl',
        Currency: 'zł'
      },
       {
        Name: 'Portugal',
        Code: 'pt',
        Currency: '€'
      },
      ],
      countrySelected: {
        Name: 'Deutschland',
        Code: 'de',
        Currency: '€'
      },
      productNumberInput: "",
      ratings: [
        {
          Quality: "",
          Nutzen: 0,
          Teilsortiment: "",
        },
      ],
      showProduct: false,
      product: undefined,
      image: "",
      price: null,
      description: "",
      features: [],
      featuresFive: [],
      showAlternatives: false,
      data: jsonData,
      options: [],
      matrixNutzen: [1, 2, 3, 4, 5],
      matrixQuality: ["A", "B", "C", "D", "E"],
      matrix: [],
      alternatives: [],
      five: [],
      four: [],
      three: [],
      two: [],
      one: [],
      loadAlternatives: false,
    };
  },
  components: {
  },
  created(){
    
  },
  methods: {
    async getInput(alternative) {
      this.productNumberInput = this.$refs.productNumberInput.value;
      console.log(this.productNumberInput);
      if(alternative){
        this.productNumberInput = alternative;
      }
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
      this.showAlternatives = false;
      this.loadAlternatives = false;
      this.five.splice(0)
      this.four.splice(0)
      this.three.splice(0)
      this.two.splice(0)
      this.one.splice(0)
      const data = await fetch(
        //search = ID Search
        //search_suggest = Vorschläge
        //productCockpitSearch = return ID Alternativen
        `http://mybackend.com:8080/shop/api/shops/www.kaiserkraft.${this.countrySelected.Code}/search?query=${this.productNumberInput}&productsPerPage=24&page=1&customerId=www.kaiserkraft.${this.countrySelected.Code}-userpricegroup&sessionId=06a9554d-8224-413d-a2ea-98a5659284eb&channelId=kkeu_de_DE&sort=`,
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
        `http://mybackend.com:8080/shop/api/shops/www.kaiserkraft.${this.countrySelected.Code}/product/${productMaster}/?articleNumber=${articleNumber}?lang=de`,
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
        this.features = this.product.classifications[0].features;
        console.log(this.features);
        this.showProduct = true;
        this.showAlternatives = false;
      }
      const result = this.data.Sheet1.find((item) => item.ID === articleNumber);
      console.log(result);

      this.ratings.Quality = result.Quality;
      this.ratings.Nutzen = result.Nutzen;
      this.ratings.Teilsortiment = result.Teilsortiment;

      this.matrix.splice(0);
      this.options.splice(0);
      this.alternatives.splice(0);

      for (let i = 0; i < this.data.Sheet1.length; i++) {
        let obj = this.data.Sheet1[i];

        if (obj.Type === result.Type) {
          this.options.push(obj);
        }
      }
      console.log(this.options);
      for (let i = 0; i < this.matrixNutzen.length; i++) {
        for (let j = 0; j < this.matrixQuality.length; j++) {
          this.matrix.push(
            this.options.find(
              (item) =>
                item.Nutzen == this.matrixNutzen[i] &&
                item.Quality == this.matrixQuality[j]
            )
          );
        }
      }
      console.log(this.matrix);
      let promiseList = [];
      
      for (let i = 0; i < this.matrix.length; i++) {
        if (!this.matrix[i]) {
          promiseList.push(Promise.resolve({brandedProductName: "Keine Treffer", galleryList:[{url: null}], price: {net: 0}}));
          //this.alternatives.push();
        } else {
          promiseList.push(
          fetch(
            `http://mybackend.com:8080/shop/api/shops/www.kaiserkraft.${this.countrySelected.Code}/product/${this.matrix[i].Master}/?articleNumber=${this.matrix[i].ID}?lang=de`,
            requestOptions
          )
            .then((response) => response.json())
            .catch((error) => {
              console.log("error", error);
            }));
          //alternativeResult.articleNumber = this.matrix[i].ID;
          // this.alternatives.push(alternativeResult);
        }
      }
      await Promise.all(promiseList);
      let responses = await Promise.all(promiseList)

      this.alternatives = responses.map((response, index) => {
         
        if (this.matrix[index]){
            response.articleNumber = this.matrix[index].ID;
        }
        return response;
        })
      
      this.loadAlternatives = true;
      this.alternatives.reverse();

      this.five.push(this.alternatives.splice(0, 5))
      this.five = this.five[0].reverse();
      console.log(this.five)
      
      this.four.push(this.alternatives.splice(0, 5))
      this.four = this.four[0].reverse();
      console.log(this.four)
      
      this.three.push(this.alternatives.splice(0, 5))
      this.three = this.three[0].reverse();
      console.log(this.three)

      this.two.push(this.alternatives.splice(0, 5))
      this.two = this.two[0].reverse();
      console.log(this.two)

      this.one.push(this.alternatives.splice(0, 5))
      this.one = this.one[0].reverse();
      console.log(this.one)

    },
    toggleAlternatives() {
      this.showAlternatives = !this.showAlternatives;
      localStorage.setItem(STORAGE_KEY, this.showAlternatives)
    },
    methodToRunOnSelect(payload) {
            this.object = payload;
          }
  },
};
</script>
