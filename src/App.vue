<template>
  <div id="app">
    <h1>Workshop 3</h1>
    <hr>
    <div class="container">
      <div class="row">
        <div class="col-md-3" v-for="item in products" :key="item.id">
          <div class="card h-100">
            <img :src="item.image" class="card-img-top">
            <div class="card-body">
              <h4 class="card-title">{{item.name}}</h4>
              <p class="card-text">ราคา {{item.price}} บาท</p>
            </div>
            <div class="card-footer">
              <button class="btn btn-primary" @click="addCart(item)">
                หยินลงตระกร้า
              </button>
            </div>
          </div>
        </div>
        <div class="col-md-6" v-if="carts.length > 0">
          <h4><i class="far fa-shopping-cart"></i> ตระกร้าสินค้า</h4>
          <table class="table">
            <thead class="table-light">
              <tr>
                <th>ภาพ</th>
                <th>ชื่อ</th>
                <th>ราคา</th>
                <th>จำนวน</th>
                <th>รวม</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in carts" :key="item.id">
                <td><img :src="item.image" width="80px" height="100px"></td>
                <td>{{item.name}}</td>
                <td>{{item.price}}</td>
                <td>
                  <i class="fas fa-minus qty-minus" @click="minusQty(item)"></i>
                  {{item.qty}}
                  <i class="fas fa-plus qty-plus" @click="plusQty(item)"></i>
                </td>
                <td>{{item.total}}</td>
                <td>
                  <button class="btn btn-danger" @click="removeProduct(item)">
                    <i class="fas fa-trash"></i>
                    </button>
                </td>
              </tr>
            </tbody>
          </table>
          <h4 align="right">ยอดชำระเงิน {{sumTotal}} บาท</h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      products: [
        {
          id: 1,
          name: "กาแฟเย็น",
          price: 50,
          image: "https://cdn.pixabay.com/photo/2014/11/21/15/20/coffee-540653__340.jpg",
          active: false
        },
        {
          id: 2,
          name: "ชามะนาว",
          price: 45,
          image: "https://cdn.pixabay.com/photo/2014/12/11/03/12/lemon-tea-563806_960_720.jpg",
          active: false
        }
      ],
      carts: [],
      coffee: 0,
      tea: 0,
    }
  },
  methods: {
    addCart:function(item){
      const index = this.carts.findIndex(o => o.id === item.id);
      if(index === -1){
        this.carts.push({
          id: item.id,
          name:item.name,
          price: item.price,
          image: item.image,
          qty: 1,
          total: item.price*1
        })
      }else{
        this.carts[index].qty += 1
        this.carts[index].total = this.carts[index].qty * this.carts[index].price
      }
    },
    removeProduct:function(item){
      const index = this.carts.indexOf(item)
      this.carts.splice(index, 1)
    },
    minusQty:function(item){
      item.qty -= 1
      if(item.qty <= 1){
        item.qty = 1
      }
      item.total = item.price * item.qty
    },
    plusQty:function(item){
      item.qty += 1
      item.total = item.price * item.qty
    }
  },
  computed: {
    sumTotal:function(){
      let sum = this.carts.reduce((prev, obj) => prev + obj.total, 0)
      return sum
    }
  }
}
</script>

<style scoped>
  .qty-minus{
    cursor: pointer;
    margin-right: 20px;
  }
  .qty-plus{
    cursor: pointer;
    margin-left: 20px;
  }
</style>