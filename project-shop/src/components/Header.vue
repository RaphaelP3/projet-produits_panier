<script>
export default {
    props: ['cart'],
    computed:{
        totalProducts(){
            return this.cart.reduce((total, product) => {
                return total + product.quantity
            }, 0)
        }
    }
}
</script>

<template>
    <div class="header">
        <router-link to="/" class="h1">Mon E-shop</router-link>
        <div>
            <slot></slot>
        </div>
        <div>
            <router-link to="/cart">
                <span class="cart">🛒</span>
            </router-link>
            <span v-if="totalProducts > 0" class="add-cart">{{ totalProducts }}</span>
        </div>
    </div>
</template>

<style>
.header {
    display: flex;
    width: 100%;
    justify-content: space-around;
    align-items: center;
    border-bottom: 1px solid black;
}
.h1{
    text-decoration: none;
    color:inherit;
    font-size: x-large;
    font-weight: 900;
}
.cart {
    font-size: x-large;
    cursor: pointer;
}
.add-cart {
    color: white;
    padding: 0.2rem;
    border-radius: 20px;
    background: red;
}
</style>