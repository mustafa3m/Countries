<template>
<div class="countries">
      <div class="country" v-for="(country, index) in countries" :key="index">
        <div class="country-img">
          <img :src="country.flags.png" />
        </div>
        <div class="country-info">
          <h5>{{ country.name }}</h5>
          <strong
            ><p>Aera : {{ country.area }}</p></strong
          >
          <strong
            ><p>Population : {{ country.population }}</p></strong
          >
          <strong
            ><p>
              Capital :
              {{ country.capital }}
            </p></strong
          >
          <strong
            ><p>Continent : {{ country.region }}</p></strong
          >
          <a @click.prevent="showCountry(country.alpha2Code)" href="#"
            >Show the Country</a
          >
        </div>
      </div>


</template>;

<script>

export default {};

</script>

<style></style>