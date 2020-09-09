<template>
  <div>
    {{$route.params}}
     <h1>{{produit.name}}</h1>
     <img :src="produit.images[0].src" width="40%">
     <p>{{produit.price}}</p>
      <pre>{{produit}}</pre>
  </div>
</template>

<script>

export default {
  name: 'Produit',
  // data () {
  //   return { 
  //   }
  // },
  computed: {
    produit() {
      let produit = localStorage.getItem('produit-' + this.$route.params.slug)
      produit = JSON.parse(produit)
      return produit
    }
  },

};
</script>
