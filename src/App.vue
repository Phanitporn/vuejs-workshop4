<template>
  <div id="app">
      <div class="container">
        <h2 align="center">Workshop #4 - ระบบตะกร้าสินค้า</h2>
        <hr>
        <div class="row">
            <div class="col-md-2" v-for="item in products">
              <div class="card h-100">
                    <img :src="item.image" class="card-img-top">
                    <div class="card-body">
                          <h4 class="card-title">{{item.name}}</h4>
                          <p class="card-text">ราคา {{item.price}} บาท</p>
                    </div>
                    <div class="card-footer">
                          <button class="btn btn-primary" @click="addCart(item)">หยิบลงตะกร้า</button>
                    </div>
              </div>
            </div>
            <div class="col-md-8" v-if="carts!=0">
              <h4>
                ตะกร้าสินค้า
                <i class="fa fa-shopping-cart"></i>
              </h4>
              <hr>
              <table class="table">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">ภาพ</th>
                  <th scope="col">ชื่อ</th>
                  <th scope="col">ราคา</th>
                  <th scope="col">จำนวน</th>
                  <th scope="col">ยอดรวม</th>
                  <th scope="col">ลบ</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="product in carts">
                    <td>
                      <img :src="product.image" alt="" width="80px" height="100px">
                    </td>
                    <td>{{product.name}}</td>
                    <td>{{product.price}}</td>
                    <td>
                      <i class="fa fa-minus qty-minus" @click="minusQty(product)"></i>
                      {{product.qty}}
                      <i class="fa fa-plus qty-plus" @click="plusQty(product)"></i>
                      </td>
                    <td>{{product.total}}</td>
                    <td>
                      <button @click="removeProduct(product)" class="btn-danger">
                          <i class="fa fa-trash"></i>
                      </button>
                    </td>
                </tr>
              </tbody>
             </table>
             <h4 align="right">ยอดชำระเงิน {{total()}} บาท</h4>
            </div>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      carts:[],
      coffee:0,
      tea:0,
      products:[
        {
          id:1,
          name:"กาแฟเย็น",
          price:30,
          image:"https://cdn.pixabay.com/photo/2017/06/21/01/17/coffee-2425652_960_720.jpg",
          active:false
        },
        {
          id:2,
          name:"ชาเย็น",
          price:35,
          image:"https://cdn.pixabay.com/photo/2015/08/26/11/20/beverage-908356_960_720.jpg",
          active:false
        }
      ]
    }
  },
  methods:{
      addCart:function(item){
          if(item.id==1){
            // ซื้อกาแฟ
            this.coffee+=1;
            if(this.coffee<=1){
             //ซื้อรอบเดียว
              this.pushData(item)
            }else{
             //รายการเดียวกัน
              var found = this.findIndex(item);
              this.carts[found].qty+=1;
              this.carts[found].total=this.carts[found].qty * this.carts[found].price;
            }
          }else{
            //ซื้อชาเย็น
            this.tea+=1;
            if(this.tea<=1){
             //ซื้อรอบเดียว
              this.pushData(item)
            }else{
             //รายการเดียวกัน
              var found = this.findIndex(item);
              this.carts[found].qty+=1;
              this.carts[found].total=this.carts[found].qty * this.carts[found].price;
            }
          }
      },
      pushData(item){
         this.carts.push({
              id:item.id,
              name:item.name,
              price:item.price,
              image:item.image,
              qty:1,
              total:item.price
          })
      },
      findIndex:function(item){
          for(var i = 0;i<this.carts.length;i++){
             if(this.carts[i].id == item.id){
                return i; //ตำแหน่งสินค้าที่ค้นเจอในตะกร้า
             }
          }
          return -1;
      },
      minusQty:function(product){
          product.qty-=1;
          if(product.qty<=1){
              product.qty=1
          }
          product.total=product.price*product.qty;
      },
      plusQty:function(product){
          product.qty+=1;
          product.total=product.price*product.qty;
      },
      removeProduct(product){
          if(confirm("คุณต้องการลบหรือไม่ ?")){
              var index=this.carts.indexOf(product);
              this.carts.splice(index,1);
              if(product.id==1){
                 this.coffee=0;
              }else{
                  this.tea=0;
              }
          }
      },
      total:function(){
         var sum=0;
         this.carts.forEach(function(item){
            sum+=item.total;
         })
         return sum;
      }
  }
}
</script>
<style  scoped>
.qty-minus{
    cursor: pointer;
    margin-right: 20px;
}
.qty-plus{
    cursor: pointer;
    margin-left: 20px;
}

</style>

