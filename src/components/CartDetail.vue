<script setup>

import { ref } from 'vue';
import  { useProductStore1 } from '@/stores/counter'
import  { useProductStore2 } from '@/stores/counter'
import  { useProductStore3 } from '@/stores/counter'
import  { useProductStore4 } from '@/stores/counter'
import { computed, watchEffect } from 'vue'


const name = ref('');
const address = ref('');
const phoneNumber = ref('');

const storeProduct1 = useProductStore1()
const storeProduct2 = useProductStore2()
const storeProduct3 = useProductStore3()
const storeProduct4 = useProductStore4()


function formatNumberWithCommas(number) {
    const formattedNumber = Number(number).toFixed(2);
    return formattedNumber.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");   
}

const totalOverallPrice = computed(() => {
  return storeProduct1.CartList.reduce((total, product) => total + product.totalProductPrice, 0)
  + storeProduct2.CartList.reduce((total, product) => total + product.totalProductPrice, 0)
  + storeProduct3.CartList.reduce((total, product) => total + product.totalProductPrice, 0)
  + storeProduct4.CartList.reduce((total, product) => total + product.totalProductPrice, 0)
})

watchEffect(() => {
  for (const product of storeProduct1.CartList) {
    product.totalProductPrice = product.quatity * product.PriceCal
  }
  for (const product of storeProduct2.CartList) {
    product.totalProductPrice = product.quatity * product.PriceCal
  }
  for (const product of storeProduct3.CartList) {
    product.totalProductPrice = product.quatity * product.PriceCal
  }
  for (const product of storeProduct4.CartList) {
    product.totalProductPrice = product.quatity * product.PriceCal
  }
})

const removeFromCart = (productId) => {
  storeProduct1.removeFromCart(productId);
  storeProduct2.removeFromCart(productId);
  storeProduct3.removeFromCart(productId);
  storeProduct4.removeFromCart(productId);
};


const placeOrder = () => {
    const hasInvalidQuantity = storeProduct1.CartList.some(product => product.quatity <= 0)
        || storeProduct2.CartList.some(product => product.quatity <= 0)
        || storeProduct3.CartList.some(product => product.quatity <= 0)
        || storeProduct4.CartList.some(product => product.quatity <= 0);

    if (hasInvalidQuantity) {
        alert("จำนวนสินค้าต้องมากกว่า 0");
    } else {
        const orderData1 = {
            orderNumber: storeProduct1.OrderList.length + 1,
            CartList: storeProduct1.CartList,
            Total: storeProduct1.totalOverallPrice,
            name: name.value,
            address: address.value,
            phoneNumber: phoneNumber.value,
        };

        const orderData2 = {
            orderNumber: storeProduct2.OrderList.length + 1,
            CartList: storeProduct2.CartList,
            Total: storeProduct2.totalOverallPrice,
            name: name.value,
            address: address.value,
            phoneNumber: phoneNumber.value,
        };

        const orderData3 = {
            orderNumber: storeProduct3.OrderList.length + 1,
            CartList: storeProduct3.CartList,
            Total: storeProduct3.totalOverallPrice,
            name: name.value,
            address: address.value,
            phoneNumber: phoneNumber.value,
        };

        const orderData4 = {
            orderNumber: storeProduct4.OrderList.length + 1,
            CartList: storeProduct4.CartList,
            Total: storeProduct4.totalOverallPrice,
            name: name.value,
            address: address.value,
            phoneNumber: phoneNumber.value,
        };

        alert("สั่งซื้อสำเร็จ! โปรดรอคนขับของเราสักครู่!");
        storeProduct1.addOrder(orderData1);
        storeProduct2.addOrder(orderData2);
        storeProduct3.addOrder(orderData3);
        storeProduct4.addOrder(orderData4);
    }
}

</script>

<template>
  <div class="container">
    <div class="p-4 p-md-5 mb-4 rounded text-body-emphasis bg-body-secondary">
          <div v-if="storeProduct1.CartList.length === 0 && storeProduct2.CartList.length === 0 && storeProduct3.CartList.length === 0 && storeProduct4.CartList.length === 0" class="incompletecart mb-3">
            <h2>อาหารของคุณ</h2>
            <div class="space-con" >
                <div class="maincontainer mt-3">
                    <div class="bd-example">
                        <h4 style="color: white; padding-top: 35vh; padding-bottom: 35vh; text-align: center; background-color: #40e87d; border-radius: 30px;">
                            ไม่มีรายการอาหารที่สั่ง
                        </h4>
                    </div>
                </div>
            </div>
            
        </div>


        <div v-else class="buying-cart">
            <div class="space-con" >
                <div class="maincontainer mt-3">
                    <div class="cart-nav">
                        <h2>ตะกร้าสินค้า</h2>
                    </div>

                    <div class="bd-example">
                    <table class="table custom-table"  style="text-align: center; vertical-align: middle;">
                        <thead>
                        <tr>
                        <th scope="col" style="width:40%;">สินค้า</th>
                        <th scope="col" style="width:10%;">ราคาต่อชิ้น</th>
                        <th scope="col" style="width:20%;">จำนวน</th>
                        <th scope="col" style="width:10%;">ราคารวม</th>
                        <th scope="col" style="width:20%;">Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(productData, index) in [...storeProduct1.CartList, ...storeProduct2.CartList, ...storeProduct3.CartList, ...storeProduct4.CartList]" :key="index">
                        <td class="product-grid">
                            <div class="product-image">
                            <img :src="productData.img" alt="" style="border-radius: 10px; width: 100%;">
                            </div>
                            <div class="product-name">
                            <span>{{ productData.Name }}</span>
                            </div>
                        </td>
                        <td>{{ productData.Price }}</td>
                        <td>
                            <div class="panelbuttcon">
                                <div class="buttcon">
                                    <div class="butsemicon">
                                    <div class="input-group mb-2">
                                        <span class="input-group-text quatitybuttbgminus">
                                        <button class="quatitybutt" @click="storeProduct1.decrementQuantity(productData)">
                                            <img class="imgbutt" src="@/assets/minus.png" alt="minus">
                                        </button>
                                        </span>
                                        <input class="form-control no-spinners" type="number" min="0" v-model="productData.quatity" style="text-align: center; font-weight: bold; " >
                                        <span class="input-group-text quatitybuttbgplus">
                                        <button class="quatitybutt" @click="storeProduct1.incrementQuantity(productData)">
                                            <img class="imgbutt" src="@/assets/plus.png" alt="minus">
                                        </button>
                                        </span>
                                    </div>
                                    </div>
                                </div>
                            </div>
                        </td>
                        <td>{{ formatNumberWithCommas(productData.totalProductPrice) }} บาท</td>
                        <td>
                            <button class="binbutt" @click="removeFromCart(productData.id)">
                                <img src="@/assets/bin.png" alt="bin" style="width: 20%;">
                                ลบออกจากตะกร้า
                            </button>
                        </td>
                        </tr>
                        
                    </tbody>

                    </table>
                    </div>
                </div>
            </div>
            

            <div class="information-bill">
                <div class="mainbill mt-3">
                    <div class="detail-bill">
                        <p>รวม : {{ formatNumberWithCommas(totalOverallPrice) }} บาท</p>
                        <hr>
                        <p>รายละเอียดการสั่งซื้อ</p>
                        <form action="" @submit.prevent="placeOrder">
                            <div class="input-group mb-3">
                                <span class="input-group-text" id="basic-addon1">ชื่อผู้รับ</span>
                                <input id="name" type="text" v-model="name" class="form-control" placeholder="ชื่อผู้รับ" aria-label="Username" aria-describedby="basic-addon1" required="">
                            </div>
                            <div class="input-group mb-3">
                                <span class="input-group-text">ที่อยู่</span>
                                <textarea class="form-control" v-model="address" aria-label="With textarea" placeholder="ที่อยู่" required></textarea>
                            </div>
                            <div class="input-group mb-3">
                                <span class="input-group-text" id="basic-addon1">เบอร์โทรศัพท์</span>
                                <input id="num" class="form-control" v-model="phoneNumber" type="text" pattern="[0-9]{10}" maxlength="10" placeholder="กรอกเบอร์โทรศัพท์(10หลัก)" aria-label="tel" aria-describedby="basic-addon1" required="">
                            </div>
                            <div class="panelbuttcon">
                                    <input type="submit" class="btn btn-success" style="margin: auto; margin-bottom: 1%;" value="ยืนยันการสั่งซื้อ" >
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<style scoped>


.space-con{
    display: flex;
    
}
.information-bill{
    display: flex;
    margin-bottom: 2%;
}

.maincontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: white;
}

.mainbill{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin-left: 58px;
    background-color: white;
}



.detail-bill{
    color: rgb(11, 34, 57);
    text-align: center;
}

.detailbill{
    color: rgb(11, 34, 57);
    text-align:left;
    font-size: larger;
}

.table td {
    padding: 0.2rem;
}

.product-grid {
  display: grid;
  grid-template-columns: 30% 70%;
  grid-column-gap: 2%; 
  align-items: center; 
}

.imgbutt{
  width: 1vw;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 70%;
  
}

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

.quatitybuttbgminus:hover{
background-color: rgb(248, 79, 49);
}

.quatitybuttbgplus:hover{
background-color: rgb(35, 197, 82);
}


.quatitybutt{
  border: hidden;   
  background-color: transparent;
}


.custom-table {
  background-color: rgb(240, 240, 240); 
  border-collapse: collapse; 
  
}


.custom-table th {
  background-color: #40e87d; 
  color: rgb(255, 255, 255); 
}


.custom-table td {
  background-color: rgb(236, 236, 236); 
  color: #333; 
  border: 1px solid rgb(230, 221, 221);
   
}


.binbutt{
  border-radius: 10px;
  padding: 2%;
  border: hidden;   
  background-color: transparent;
  background-color: rgb(255, 255, 255);
}

.binbutt:hover{
  border: hidden;   
  background-color: rgb(248, 79, 49);
}

.panelbuttcon{
    display: flex;
}
</style>