<script>
export default {
  data(){
    return{
      cart:[],
      notification:''
    }
  },
  mounted(){
  const saved = localStorage.getItem("cart")
  if(saved){
    this.cart = JSON.parse(saved)
  }
},
  watch:{
    cart:{
      handler(newCart){
        localStorage.setItem("cart",JSON.stringify(newCart))
      },
      deep:true
    }
  //handler avec deep permettent de voir les changements de l'objet dans le panier
  },
  methods:{
    toast(alerte){
      this.notification = alerte
      setTimeout(()=>{
        this.notification = ""
      },2000)
    }
  }
}
</script>

<template>
  <router-view 
  :cart="cart"
  :toast="toast"
  />

  <!-- Alerte notification -->
  <div v-if="notification" class="toast">
    {{ notification }}
  </div>
</template>

<style>
html, body {
  overflow-x: hidden;
  width: 100%;
}
.toast{
  display: flex;
  justify-content: center;
  position: fixed;
  width: 100%;
  height: 50px;
  top: 0;
  left: 0;
  background: #FD6301;
  color: white;
  font-size: 30px;
  padding: 1rem;
}
</style>