<script>
import Header from '@/components/Header.vue';
export default {
  components:{
    Header
  },
  props: ['cart','toast'],
  data() {
    return {
      count: 0,
      search: "",
      modal: false,
      newProduct: null,
      products: []
    }
  },
  mounted() {
  fetch("https://fakestoreapi.com/products")
    .then(res => res.json())
    .then(data => {
      this.products = data.map(product => ({
        ...product,
        quantity: 0
      }))
    })
},
  methods: {
    addProduct(product) {
      this.newProduct = product
      this.newProduct.quantity = 1
      this.modal = true
    },
    addCart(product) {
      this.cart.push(product)
      this.modal = false
      this.toast("Ajouté au panier !")

    },
    add(item) {
      item.quantity++
    },
    remove(item) {
      if (item.quantity > 1) {
        item.quantity--
      }
    }
  },
  computed: {
    filteredProducts() {
      return this.products.filter(product =>
        product.title.toLowerCase().includes(this.search.toLowerCase())
      )

    }
  }
}
</script>

<template>
  <div class="body">
    <!-- Header -->
  <Header :cart="cart">
    <input 
      class="input"
      placeholder="Rechercher"
      v-model="search"
    />
  </Header>

    <!-- Produits -->
    <div class="main">
      <div class="section">
        <div class="card" v-for="product in filteredProducts" :key="product.id">
          <div class="item">{{ product.title }}</div>
          <img :src="product.image">
          <p class="p">{{ product.price }} €</p>
          <button class="btn" @click="addProduct(product)">
            Ajouter au panier
          </button>
        </div>
      </div>
    </div>

    <!-- Modal produit -->
    <div class="modal" v-if="modal">
      <div class="modal-content">
        <h2>{{ newProduct.title }}</h2>
        <img class="img" :src="newProduct.image">
        <p class="p">{{ newProduct.price }} €</p>
        <div class="quantity">
          <button @click="remove(newProduct)">-</button>
          <span>{{ newProduct.quantity }}</span>
          <button @click="add(newProduct)">+</button>
        </div>
        <button class="btn" @click="addCart(newProduct)">
          Ajouter au panier
        </button>
        <button class="btn-close" @click="modal = false">
          X
        </button>
      </div>
    </div>
  </div>
</template>

<style>

.body {
  width: 100%;
  display: flex;
  flex-direction: column;
}
.header {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  justify-content: space-around;
  align-items: center;
  border-bottom: 1px solid black;
  padding: 1rem;
  background-color: white;
}
.input {
  width: 150px;
  height: 10px;
  font-size: medium;
  border-radius: 20px;
  padding: 0.5rem;
}
.cart {
  font-size: x-large;
  cursor: pointer;
}
.main {
  display: flex;
  flex-direction: column;
  margin-top: 80px;
  width: 100%;
  padding: 1rem;
  gap: 1rem;
}
.section {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}
.card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 300px;
  gap: 2rem;
  border: 1px solid black;
  padding: 1rem;
}
.card img {
  width: 150px;
}
.item{
    font-size: large;
    font-weight: 600;
  }
.p{
  font-size: large;
  font-weight: 700;
}
.btn {
  width: 150px;
  border-radius: 20px;
  padding: 0.5rem;
  border: none;
  background-color: rgb(255, 225, 0);
  cursor: pointer;
}
.add-cart {
  color: white;
  background-color: red;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.479);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}
.modal-content {
  background: white;
  padding: 2rem;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  width: 600px;
  height: 650px;
}
.btn-close{
  display: flex;
  padding: 0.2rem;
  width: 50px;
  justify-content: center;
  align-items: center;
  background-color: red ;
  color: white;
  font-size: medium;
  position: fixed;
  top: 0;
  right: 1px;
  cursor: pointer;
}
</style>