<template>
  <div class="home">
    <div class="produit" v-for="produit in produits" v-if="display" :key="produit.id"> 
      <img v-if="produit.images[0]" :src="produit.images[0].src" width="100%">
      
      <div>{{produit.name}}</div>

      <router-link :to="'/produit/' + produit.slug">Fiche produit</router-link>

  </div>
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

// Option 1
// Ref : https://stackoverflow.com/questions/45463628/woocommerce-rest-api-oauth-with-javascript/45465247#45465247
import axios from 'axios'
import OAuth  from 'oauth-1.0a'
import CryptoJS from 'crypto-js'
import jQuery from 'jquery'

const that = this

const ck = 'ck_50fe0809f46d9257eea67e3dc2f7cd25c577c776'
const cs = 'cs_e74730bd667bed144fa25c6fda330d99b0e949b6'
const url = 'http://52.47.179.249/wp-json/wc/v3/products'

const oauth = OAuth({
    consumer: {
        key: ck,
        secret: cs
    },
    signature_method: 'HMAC-SHA1',
    hash_function: function(base_string, key) {
        return CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA1(base_string, key));
    }
});

const requestData = {
    url: url,
    method: 'GET'
};




// 


export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  mounted () {
    let vm = this
    // Optimisations : ne pas relancer la requête si récupérée il y a moins de X minutes.
    axios.get(
    requestData.url + '?' + jQuery.param(oauth.authorize(requestData))
      ).then(function(response){
          console.log(response.data)
          vm.produits = response.data
          vm.produits.forEach(p => localStorage.setItem('produit-' + p.slug, JSON.stringify(p))
          );
          vm.display = true
      }, function(error){
          console.log(error)
      })
  },
  data () {
    return {
      produits: [],
      display: false
    }
  }

};
</script>
<style>
.home {
  display: flex;
  flex-wrap: wrap;
}
.produit {
  width: 20vw;
  margin-bottom: 2em
}
</style>
