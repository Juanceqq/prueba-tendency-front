<template>
    <div class="p-5 h-full bg-gray-100 flex flex-col gap-10">
    <h1 class="text-xl px-20">Mis ordenes </h1>

    <div class="px-24">
      <table class="w-full ">
      <thead class="bg-gray-200 border-2 border-gray-200">
        <tr class="text-sm font-semibold tracking-wide text-left">
          <th class="px-3 py-5">No.</th>
          <th class="px-3 py-5">Orden Realizada</th>
          <th class="px-3 py-5">Items</th>
          <th class="px-3 py-5">Total</th>
        </tr>
      </thead>
      <tbody>
        <tr class="odd:bg-white even:bg-slate-50 text-sm text-gray-700 " v-for="p in products.orders" :key="p.id">
          <td class="p-4 font-bold border-x-2">
            
            <RouterLink :to="{
                name: 'OrderView',
                params: {
                  number: p.number.toString(),
                },
              }"
            class="w-16 text-blue-500 hover:underline">{{ `#${p.number}` }}
            </RouterLink>
          </td>
          <td class="p-4 border-r-2">{{ formatDate(p.dates.createdAt) }}</td>
          <td class="flex p-4 border-r-2" v-for="item in p.items" :key="item.id">
            <p>{{ item.name }}</p> 
          </td>
          <td class="p-4 border-r-2">{{ `$ ${p.totals.total}` }}</td>
        </tr>
      </tbody>
    </table>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return{
      products:[]
    }
  },
  methods:{
    formatDate: function (date){
      let timestamp = new Date(date).getTime()
      let day = new Date(timestamp).getDate()
      let month = new Date(timestamp).getMonth()
      let year = new Date(timestamp).getFullYear()
      let dateFormat = `${day}/${month}/${year}`
      return dateFormat
    },

    orderRouteTo: function(number){
      return `/orders/${number}`
    }
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
