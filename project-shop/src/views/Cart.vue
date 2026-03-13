<script>
import Header from '@/components/Header.vue';
export default {
  components:{
    Header
  },
  name:"Cart",
  props:["cart", 'toast'],
  methods:{
    add(item){
      item.quantity++
    },
    remove(item){
      if(item.quantity > 1){
        item.quantity--
      }
    },
    deleteProduct(item){
        const index = this.cart.indexOf(item)
        this.cart.splice(index,1)
        this.toast("Retiré du panier !")

    },
    deleteCart(){
        this.cart.splice(0)
        this.toast("Le panier a été vidé ! ")
    }
  },
  computed:{
    total(){
      return this.cart.reduce((sum,item)=>{
        return sum + item.price * item.quantity
      },0)
    }
  }
}
</script>

<template>
<div class="body">
<Header :cart="cart" />

  <!-- Ajouter le header -->
    <h2>Panier</h2>
    <div class="title-empty-cart" v-if="cart.length === 0">
        Panier vide
    </div>

    <div class="cart-content" v-for="item in cart" :key="item.id">
        <div>
          <p class="item-price">{{ item.title }}</p>
          <img class="img" :src="item.image">
        </div>
        <div class="item-option">
          <div class="item-price">Prix : {{ item.price }} €</div>
          <div class="quantity">
              <button class="btn-add-remove-delete" @click="remove(item)">-</button>
              <span class="item-quantity">{{ item.quantity }}</span>
              <button class="btn-add-remove-delete" @click="add(item)">+</button>
              <button class="btn-add-remove-delete" @click="deleteProduct(item)">🗑</button>
          </div>  
        </div>
    </div>
    <div v-if="cart.length >=1 " class="info-cart"> 
      <p>Total panier : {{ total }} €</p>
      <button class="btn-deleteCart" @click="deleteCart">Vider le panier</button>
    </div>
</div>
</template>

<style>
  .body{
    display: flex;
    justify-content: center ;
    gap: 1rem;
  }
  .title-empty-cart{
    display: flex;
    justify-content: center;
    font-size: x-large;
    font-weight: 700;
  }
  .cart-content{
    display: flex;
    width: 800px;
    border: 1px solid black;
    padding: 1rem;
    gap: 8rem;
  }
  .item-price{
    font-size: large;
    font-weight: 600;
  }
  .img{
    width: 300px;
  }
  .item-option{
    display: flex;
    flex-direction: column;
    gap: 3rem;
    margin-top: 1rem;
  }
  .quantity{
    display: flex;
    gap: 2rem;
    align-items: center;
  }
  .btn-add-remove-delete{
    width: 30px;
    height: 30px;
    font-size: large;
  }
  .item-quantity{
    font-size: x-large;
    font-weight: 400;
  }
  .info-cart{
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 100px;
    right: 100px;
    gap: 1rem;
    font-size: x-large;
    font-weight: 600;
  }
  .btn-deleteCart{
    width: 200px;
    border-radius: 10px;
    border: none;
    padding: 0.5rem;
    background-color: rgb(255, 157, 0);
    font-size: large;
    cursor: pointer;
  }
</style>