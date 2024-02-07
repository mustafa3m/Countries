<template>
  <main>
    <div class="modal" v-if="showModal">
      <!-- Modalt innhold -->
      <div class="modal modal-wrapper ">
        <div class="country center wrapper ">
          <div class="country-info">
            <a  class="country-close" @click.prevent="showHideModal" href="#">close</a>
            <div class="country-img">
              <img class="country-margin-top" :src="country[0].flags.png" />
            </div>
            <h5> {{country[0].name }}</h5><strong><p>Aera : {{ country[0].area }}</p></strong><strong><p>Population : {{ country[0].population }}</p></strong>
            <strong><p>Capital :{{ country[0].capital }}</p></strong>
            <strong><p>Continent : {{ country[0].region }}</p></strong>
          </div>
        </div>
        

       
      </div>
    </div>

    <h1>Countries of the World</h1>

    <div class="filter-region">
      <button @click="filterCountries('all')">All Countries</button>
      <button @click="filterCountries('Africa')">Africa</button>
      <button @click="filterCountries('Americas')">America</button>
      <button @click="filterCountries('Asia')">Asia</button>
      <button @click="filterCountries('Europe')">Europe</button>
      <button @click="filterCountries('Oceania')">Oceania</button>
    </div>

    <div class="countries">
      <div class="country" v-for="(country, index) in countries" :key="index">
        <div class="country-img">
          <img :src="country.flags.png" />
        </div>
        <div class="country-info">
          <h5>{{ country.name }}</h5>
          <strong><p>Aera : {{ country.area }}</p></strong>
          <strong><p>Population : {{ country.population }}</p></strong>
          <strong><p>Capital : {{ country.capital }} </p></strong>
          <strong><p>Continent : {{ country.region }}</p></strong>
          <a @click.prevent="showCountry(country.alpha2Code)" href="#">Show the Country</a >
        </div>
      </div>
    </div>
  </main>
</template>

<script>
  export default {
    name: "home",
    data() {
      return {
        countries: [],
        fetchedCountries: [],
        country: [],
        showTheCountry: false,
        showModal: false,
      };
    },

    created() {
      this.countriesSearch("all");
    },

    methods: {
      //Denne metoden fetch alle land
      async countriesSearch(subregion) {
        let param = "";
        if (subregion === "all") param = subregion;
        else param = `region ${subregion}`;
        const url = `https://restcountries.com/v2/${param}`;
        const res = await fetch(url);
        const result = await res.json();
        this.countries = result;
        this.fetchedCountries = result; //på den måte har vi en backup av alle land og trenger vi ikke å fetch på nytt
        console.log(result);
      },

      //Denne metoden filtrerer og returnere et land
      showCountry(code) {
        const allCountries = this.countries;
        const country = allCountries.filter(
          (country) => country.alpha2Code == code
        );
        this.country = country;
        this.showTheCountry = true;
        console.log(country);
        this.showModal = true;
      },

      // Denne metoden viser Modalt
      showHideModal() {
        this.showModal = !this.showModal;
        this.showModal = this.country.name;
      },

      //Denne metoden filtrerer og returnere  et kontinent
      filterCountries(continent) {
        if (continent && continent !== "all") {
          this.countries = this.fetchedCountries;
          const filtered = this.countries.filter((country) => {
            console.log(country.region);
            return country.region === continent;
          });
          this.countries = filtered;
        } else {
          this.countries = this.fetchedCountries; // vi oppdatere fra den variabel som har alle countries
        }
      },
    },
  };
</script>

<style scope>
  h1 {
    text-align: center;
    padding: 50px;
    color: white;
  }
  .modal {
    width: 100vw;
    height: 100vh;
    color: #fff;
    background-color: rgba(0, 0, 0, 0.6);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 5;
  }
  .modal-wrapper{
    display: flex;
  align-items: center;
  justify-content: center;

  }
  .wrapper {
  height: 50vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rebeccapurple;
 
}
.country-margin-top {
  margin-top: 0.5rem;
}
.country-close{
  color:#fff;
  text-decoration: none;
  font-size: 1.5rem;
}


  /* modal */
 
  .container {
    width: 100vw;
    height: 100vh;
    padding: 30px;
    display: flex;
    justify-content: space-between;
  }
  .countries {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    padding: 30px;
  }
  .country {
    min-height: 400px;
    width: 300px;
    box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.3);
    border-radius: 5px;
    overflow: hidden;
    margin-top: 30px;
  }
  .center {
    text-align: center;
    
  }
  .country-img {
    height: 250px;
  }
  .country-img img {
    width: 100%;
    height: 100%;
  }
  .country-info {
    padding: 20px 17px;
    letter-spacing: 1px;
  }
  .country-info h5 {
    padding: 10px 0px;
    font-size: 20px;
    letter-spacing: 1px;
  }
  .country-info p > strong {
    font-weight: 600;
  }
 

  .form-control input {
    width: 100%;
    padding: 20px 24px;
    border-radius: 20px;
    outline: 0;
    font-size: 15px;
    letter-spacing: 0.5px;
  }
  .filter-region button {
    margin-right: 20px;
  }

  button {
    border-radius: 20px;
    padding: 5px;
  }
</style>
