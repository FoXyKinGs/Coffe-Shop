<template>
  <div class="home">
    <Navbar title="Home" type="0" :data="dataCart"/>
    <div class="content">
      <!-- Content -->
        <template>
          <div class="navigasi" style="display: flex;">
          <b-pagination></b-pagination>
          <input type="text" placeholder="Search" style="position: absolute; right: 20px; height: 30px;" v-model="search" @keyup="getAllProduct()">
          <!-- <select style="position: absolute; right: 30%; height: 30px;">
            <option>ASC</option>
            <option>DESC</option>
          </select> -->
          </div>
          <div class="row mt-3">
            <div id="target" class="card col-lg-4 col-md-4 col-sm-6 col-xs-12" v-for="(element, index) in listProduct" :key="index">
              <img :src="element.image" class="card-img-top" style="width: 100% !important; height: 200px;" v-on:click="addToCart(element)">
              <router-link :to="'/detailmenu/' + element.id" class="btn btn-light border-0">Detail</router-link>
                <div class="card-body">
                  <h6 class="card-title">{{element.name}}</h6>
                  <h5>Rp.{{element.price}}</h5>
                </div>
            </div>
          </div>
        </template>
        <!-- End of content -->

        <!-- Res-Cart -->
        <div v-if="dataCart.length > 0">
          <div class="res-cart">
            <div class="menus">
              <div v-for="(items, index) in dataCart" :key="index">
              <div class="bg-light mb-3" style="width: 93%; background: #fff !important">
              <div class="row g-0">
                <div class="col-md-4">
                  <img :src="items.image" style="width: 100px;">
                </div>
                <div class="col-md-8">
                  <div class="card-body" style="margin-left: 20px;">
                    <h5 class="card-title">{{items.name}}<button class="delete" v-on:click="deleteCart(items.id)">x</button></h5>
                      <button class="auto-text" v-on:click="minQty(items.id)" style="background: #dffadb">-</button>
                      <div class="auto-text">{{items.qty}}</div>
                      <button class="auto-text" v-on:click="plusQty(items.id)" style="background: #dffadb">+</button>
                      <div class="price">Rp. {{items.price * items.qty}}</div>
                  </div>
                </div>
              </div>
            </div>
            </div>
            </div>
            <div class="cashier">
              <div class="row pl-2">
              <input class="col-lg-6" type="text" placeholder="Cashier" v-model="finalResult.cashier">
              <input class="col-lg-6" type="text" placeholder="Invoice" v-model="finalResult.invoice">
              </div>
            </div>
            <div class="information">
              <div class="info">
            <div class="prices">
              <h4>Harga:</h4>
              <h3>Rp. {{total}}</h3>
            </div>
            <div class="more-info">
              <h6>*Belum termasuk PPN</h6>
            </div>
            </div>
              <div class="buttons">
              <button type="button" class="btn btn-primary btn-lg btn-block" style="background-color:#57CAD5; border: none;" @click="$bvModal.show('modal-2'); Final()">Check Out</button>
              <button type="button" class="btn btn-secondary btn-lg btn-block" @click="deleteOrders()" style="background-color: #F24F8A; border: none;">Cancel</button>
              </div>
            </div>
          </div>
          </div>
          <div v-else>
            <div class="novalue">
              <div class="value">
                <img src="../assets/Icon/food-and-restaurant.png" alt="Empty">
                <h3>Your Cart Is Empty</h3>
                <p>please add some items from the menu</p>
              </div>
            </div>
          </div>
        <!-- End of res-cart -->
    </div>

    <!-- Cart -->
    <div class="content-cart">
      <div class="orders">
        <div v-if="dataCart.length > 0">
          <div v-for="(items, index) in dataCart" :key="index">
            <div class="bg-light mb-3" style="width: 93%; background: #fff !important">
            <div class="row g-0">
              <div class="col-md-2">
                <img :src="items.image" class="m-4" style="width: 60px;">
              </div>
              <div class="col-md-10">
                <div class="card-body" style="margin-left: 20px;">
                  <h5 class="card-title">{{items.name}}<button class="delete" v-on:click="deleteCart(items.id)">x</button></h5>
                    <button class="auto-text" v-on:click="minQty(items.id)" style="background: #dffadb">-</button>
                    <div class="auto-text">{{items.qty}}</div>
                    <button class="auto-text" v-on:click="plusQty(items.id)" style="background: #dffadb">+</button>
                    <div class="price">Rp. {{items.price * items.qty}}</div>
                </div>
              </div>
            </div>
          </div>
          </div>
          <div class="info">
            <div class="prices">
            <h4>Harga:</h4>
            <h3>Rp. {{total}}</h3>
            </div>
            <div class="more-info">
              <h6>*Belum termasuk PPN</h6>
            </div>
            <div class="cashier">
              <div class="row pl-4 pr-5">
              <input class="col-lg-6" type="text" placeholder="Cashier" v-model="finalResult.cashier">
              <input class="col-lg-6" type="text" placeholder="Invoice" v-model="finalResult.invoice">
              </div>
            </div>
          </div>
        <div class="buttons">
        <button type="button" class="btn btn-primary btn-lg btn-block" style="background-color:#57CAD5; border: none;" @click="$bvModal.show('modal-2')">Check Out</button>
        <button type="button" class="btn btn-secondary btn-lg btn-block" @click="deleteOrders()" style="background-color: #F24F8A; border: none;">Cancel</button>
        </div>
        </div>
        <div v-else>
          <div class="nothing-else">
            <img src="../assets/Icon/food-and-restaurant.png" alt="Empty">
            <h3>Your Cart Is Empty</h3>
            <p>please add some items from the menu</p>
          </div>
        </div>
      </div>
    </div>
    <!-- End of chart -->

    <!-- Modal check out-->
    <b-modal id="modal-2">
      <template #modal-header>
        <div class="row">
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-3"><h3>Checkout</h3></div>
          <div class="col-lg-8 col-md-8 col-sm-8 col-xs-9" style="position: absolute; right: 0;">Invoice #{{finalResult.invoice}}</div>
          <h6 class="col-lg-12">Cashier : {{finalResult.cashier}}</h6>
        </div>
      </template>
      <form class="mt-2">
        <div class="row" v-for="(item, index) in dataCart" :key="index">
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-4">{{item.name}} x{{item.qty}}</div>
          <div class="col-lg-8 col-md-8 col-sm-8 col-xs-8 text-right">Rp. {{item.price * item.qty}}</div>
        </div>
        <div class="row mt-4">
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-4">PPN 10%</div>
          <div class="col-lg-8 col-md-8 col-sm-8 col-xs-8 text-right">Rp. {{this.ppn = this.total * 0.1}}</div>
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-4"><h4>Total :</h4></div>
          <div class="col-lg-8 col-md-8 col-sm-8 col-xs-8 text-right"><h4>Rp. {{this.total + this.ppn}}</h4></div>
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-4"><h6>payment: cash</h6></div>
        </div>
      </form>
      <template #modal-footer>
        <div class="w-100">
        <b-button type="button" class="btn text-white border-0" style="background-color: #F24F8A;" block @click="$bvModal.hide('modal-2');deleteOrders()">Print</b-button>
        <b-button type="button" class="btn text-white border-0" style="background-color:#57CAD5;" block @click="$bvModal.hide('modal-2');deleteOrders()">Send Email</b-button>
        </div>
      </template>
    </b-modal>
    <!-- End of modal check out -->
  </div>
</template>

<script>
import Navbar from '../components/Navbar'
import Axios from 'axios'
export default {
  data () {
    return {
      search: '',
      listProduct: [],
      dataCart: [],
      total: 0,
      ppn: 0,
      Result: [],
      finalResult: {
        invoice: '',
        cashier: '',
        orders: '',
        amount: ''
      }
    }
  },
  computed: {
    rows () {
      return this.listProduct.length
    }
  },
  mounted () {
    this.getAllProduct()
  },
  components: {
    Navbar
  },
  methods: {
    addToCart: function (value) {
      const checkProduct = this.dataCart.filter((item) => {
        return item.id === value.id
      })
      if (checkProduct.length >= 1) {
        this.dataCart.forEach(element => {
          if (element.id === value.id) {
            element.qty += 1
          }
        })
      } else {
        const newData = {
          id: value.id,
          name: value.name,
          image: value.image,
          price: value.price,
          qty: 1
        }
        this.dataCart = [...this.dataCart, newData]
      }
      this.totalCart()
    },
    minQty: function (id) {
      this.dataCart.forEach(element => {
        if (id === element.id) {
          element.qty -= 1
          if (element.qty < 1) {
            element.qty = 1
          }
        }
        this.totalCart()
      })
    },
    plusQty: function (id) {
      this.dataCart.forEach(element => {
        if (id === element.id) {
          element.qty += 1
          if (element.qty > 99) {
            element.qty = 99
          }
        }
        this.totalCart()
      })
    },
    deleteOrders: function () {
      this.dataCart = []
      this.totalCart()
    },
    deleteCart: function (id) {
      const newestCart = this.dataCart.filter((item) => {
        return item.id !== id
      })
      this.dataCart = newestCart
      this.totalCart()
    },
    totalCart: function () {
      this.total = 0
      this.dataCart.forEach(element => {
        this.total += element.price * element.qty
      })
    },
    Final: function () {
      this.finalResult.orders = ''
      this.dataCart.forEach(element => {
        this.finalResult.orders += element.name + ' ' + element.qty + ','
        this.ppn = this.total * 0.1
        this.finalResult.amount = this.total + this.ppn
      })
    },
    getAllProduct: function () {
      Axios.get('http://localhost:3000/product?name=' + this.search).then((response) => {
        this.listProduct = response.data.data
      }).catch((error) => {
        console.log(error)
      })
    }
  }
}
</script>

<style scoped>
  .content{
      position: fixed;
      margin-top: 70px;
      height: calc(100% - 70px);
      width: calc(100% - 30% - 80px);
      margin-left: 80px;
      margin-right: 50px;
      padding: 20px 20px 0 20px;
      overflow: auto;
      background: #E5E5E5;
  }
  .content-cart{
    top: 70px;
    right: 0;
    height: 100%;
    width: 30%;
    position: fixed;
    padding: 10px;
  }
  .content-cart .orders{
    position: relative;
    margin-left: 10px;
    height: 60%;
    overflow: auto;
  }
  .content-cart .buttons{
    position: fixed;
    bottom: 20px;
    width: 27%;
  }
  .card{
    background-color: #e5e5e5 !important;
    border: none;
  }
  .auto-text{
    display: inline-block;
    width: 25px;
    height: 25px;
    border: 1px solid green;
    justify-content: center;
    text-align: center;
    color: #82DE3A;
  }
  .price{
    display: inline-block;
    position: absolute;
    right: 10%;
  }
  .card-img-top:hover{
    opacity: 0.5;
  }
  .content-cart .info{
    position: fixed;
    bottom: 120px;
  }
  .content-cart .info .prices{
    display: flex;
  }
  .content-cart .info .prices h3{
    position: fixed;
    right: 20px;
  }
  .nothing-else img{
    position: absolute;
    top: 10%;
    left: 20%;
  }
  .nothing-else h3{
    position: absolute;
    top: 50%;
    left: 15%;
  }
  .nothing-else p{
    position: absolute;
    top: 60%;
    left: 10%;
    color: #8f8f8f;
  }
  .delete{
    background: #F24F8A;
    position: absolute;
    right: 25px;
    width: 30px;
    transform: translate(-30%, -70%);
    border-radius: 50%;
    color: #fff;
  }
  .res-cart{
    background: white;
    margin-bottom: 30px;
    width: 100%;
    height: 700px;
    padding: 20px;
    display: none;
  }
  .res-cart .menus{
    position: relative;
    width: 100%;
    height: 70%;
    overflow: auto;
  }
  .res-cart .information .buttons{
    bottom: 0;
    position: relative;
    width: 100%;
  }
  .novalue{
    background: white;
    position: relative;
    width: 100%;
    height: 700px;
    padding: 20px;
    display: none;
  }
  .novalue .value{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  .prices{
    display: flex;
  }
  @media (max-width: 1080px){
    .content-cart{
      display: none;
    }
    .content{
      width: calc(100% - 80px);
    }
    .card-img-top{
      width: 100% !important;
      height: 200px !important;
    }
    .res-cart{
      display: inline-block;
    }
    .novalue{
      display: inline-block;
    }
  }
  @media (max-width: 765px){
    .card-img-top{
      width: 100% !important;
      height: 200px !important;
    }
  }
  @media (max-width: 577px){
    .card-img-top{
      width: 100% !important;
      height: 250px !important;
    }
  }
</style>
