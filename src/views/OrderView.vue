<template>
  <div class="p-5 h-screen bg-gray-100 flex flex-col gap-10">
    <div class="flex flex-col sm:flex-row sm:items-center gap-4 px-20">
      <div class="text-xl">
        <RouterLink class="bg-gray-200 hover:bg-gray-300 rounded-md px-6 py-3" to="/">Volver</RouterLink>
      </div>
      <div class=" flex flex-col gap-1">
        <h1 class="text-3xl">Detalles del pedido</h1>
        <p>No. de pedido: #{{ number }}</p>
      </div>
    </div>
    <div class="px-24">
      <table class="w-full">
        <thead class="bg-gray-200 border-2 border-gray-200">
          <tr class="text-sm font-semibold tracking-wide text-left">
            <th class="px-3 py-5">Sku</th>
            <th class="px-3 py-5">Name</th>
            <th class="px-3 py-5">Quantity</th>
            <th class="px-3 py-5">Price</th>
          </tr>
        </thead>
        <tbody v-for="p in products.orders" :key="p.number">
          <tr class="odd:bg-white even:bg-slate-50 text-sm text-gray-700" v-for="item in p.items" :key="item.id">
            <td class="border-x-2" v-if="p.number === number">
              <div class="flex p-4" >
                <p>{{ item.sku }}</p> 
              </div>
            </td>
            <td class="border-x-2" v-if="p.number === number">
              <div class="flex p-4" >
                <p>{{ item.name }}</p> 
              </div>
            </td>
            <td class="border-x-2" v-if="p.number === number">
              <div class="flex p-4" >
                <p>{{ item.quantity }}</p> 
              </div>
            </td>
            <td class="border-x-2" v-if="p.number === number">
              <div class="flex p-4" >
                <p> ${{ item.price }}</p> 
              </div>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="pt-4 flex justify-end">
        <button @click="showAlert" class="bg-positive hover:bg-positive-400 text-white rounded-md px-6 py-3">
          Pagar
        </button>
      </div>
    </div>
</div>
</template>

<script>
import router from '@/router'
import Swal from 'sweetalert2'

export default {
    data(){
        return{
            products: []
        }
    },
    methods: {
      showAlert() {
        Swal.fire({
        icon: 'success',
        title: 'Pago recibido',
        showConfirmButton: false,
        timer: 1500
        })
        router.push({ path: '/' })
      }
    },
    props:{
        number: {
            type: String,
            required: true
        },
    },
    mounted(){
    const headers = {
      "Authorization": 'eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJwUGFINU55VXRxTUkzMDZtajdZVHdHV3JIZE81cWxmaCIsImlhdCI6MTYyMDY2Mjk4NjIwM30.lhfzSXW9_TC67SdDKyDbMOYiYsKuSk6bG6XDE1wz2OL4Tq0Og9NbLMhb0LUtmrgzfWiTrqAFfnPldd8QzWvgVQ'
    }

    fetch("https://eshop-deve.herokuapp.com/api/v2/orders",  { headers } )
      .then(res => res.json())
      .then(response => this.products = response)
  }
}
</script>