<script setup>
import NavbarRestaurant from '../../components/NavbarRestaurant.vue';
import Band from '../../components/Band.vue';
import { saveMenuSelect } from '../../store/menu.js'
import OrderReview from '../../components/OrderReview.vue';
import { ref, onBeforeMount } from 'vue';

const menuStore = saveMenuSelect()
const orders = ref([]);
const sentBodyData = { restaurantId: menuStore._id }
const name = "Réception de " + menuStore.name;



async function getOrders() {
  try {
    const response = await fetch(import.meta.env.VITE_ENDPOINT_URL + 'order/orderid', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Token ' + localStorage.token
      },
      body: JSON.stringify(sentBodyData)
    });
    const jsonData = await response.json();
    let dataBody = jsonData.body
    if (response.ok) {
      orders.value=dataBody;
      console.log(dataBody)
    } else {
      console.log('Une erreur s\'est produite lors de la récupération des restaurants');
    }
  } catch (error) {
    console.error('Une erreur s\'est produite lors de la requête GET :', error);
  }
}

onBeforeMount( async () => {
  await getOrders()
})


</script>

<template>
    <NavbarRestaurant/>
    <Band :name=name />


    <div v-for="order in orders" :key="order.id" class="orderItem-div">
                <OrderReview 
                :restaurant="order.restaurantName" 
                :order_items="order.order_items" 
                :price="order.totalPrice" />
            </div>

</template>

<style scoped>
.orderItem-div{
    width: 80%;
    margin-top: 2%;
    padding: 1%;
    background-color: white;
    border-radius: 30px;
    color: black;
    font-weight: bold;
}

</style>
