<template>
    <div>
        <Navbar title="detailmenu" type="2"/>
        <div class="content">
            <div class="row">
                <div class="card mb-3" style="width: 100%;" v-for="(element, index) in result" :key="index">
                <div class="row g-0">
                    <div class="col-md-4">
                        <img :src="element.image" alt="..." style="width: 100% !important; height: 100%; padding: 25px">
                    </div>
                    <div class="col-md-8">
                        <div class="card-body">
                            <h5 class="card-title">Id : </h5>
                            <p class="card-text">{{element.id}}</p>
                            <h5 class="card-title">Id category : </h5>
                            <p class="card-text">{{element.id_category}}</p>
                            <h5 class="card-title">Product Name : </h5>
                            <p class="card-text">{{element.product_name}}</p>
                            <h5 class="card-title">Image : </h5>
                            <p class="card-text">{{element.image}}</p>
                            <h5 class="card-title">Price : </h5>
                            <p class="card-text">{{element.price}}</p>
                            <h5 class="card-title">Category : </h5>
                            <p class="card-text">{{element.category_name}}</p>
                        </div>
                    </div>
                </div>
            </div>
            </div>
            <form v-for="(element, index) in result" :key="' A ' + index">
                <div class="row">
                <h6 class="col-lg-3">ID :</h6><input class="form-control col-lg-9" type="text" :value="element.id" disabled>
                <h6 class="col-lg-3 mt-2">ID Category :</h6><select class="col-lg-8 mt-2" v-model="newData.id_category">
                    <option v-for="(item, index) in category" :key="' B ' + index" :value="item.id">{{item.category_name}}</option>
                </select>
                <h6 class="col-lg-3 mt-2">Product Name :</h6><input class="form-control col-lg-9 mt-2" type="text" v-model="newData.product_name" :placeholder="element.product_name">
                <h6 class="col-lg-3 mt-2">Image :</h6><input class="form-control col-lg-9 mt-2" type="text" v-model="newData.image" :placeholder="element.image">
                <h6 class="col-lg-3 mt-2">Price :</h6><input class="form-control col-lg-9 mt-2" type="text" v-model="newData.price" :placeholder="element.price">
                </div>
            </form>
            <div v-for="(element, index) in result" :key="' C ' + index">
            <button class="btn btn-primary" @click="updateProduct(element.id)">Update</button>
            <button class="btn btn-danger" @click="deleteProduct(element.id)">Delete</button>
            </div>
        </div>
    </div>
</template>
<script>
import Navbar from '../components/Navbar'
import Axios from 'axios'
export default {
  data () {
    return {
      id: this.$route.params.id,
      result: [],
      category: [],
      newData: {
        id_category: '',
        product_name: '',
        image: '',
        price: ''
      }
    }
  },
  components: {
    Navbar
  },
  methods: {
    getAllProduct: function () {
      Axios.get('http://localhost:3000/detailproduct/' + this.id).then((response) => {
        this.result = response.data.data
      }).catch((err) => {
        console.log(err)
      })
    },
    getAllCategory: function () {
      Axios.get('http://localhost:3000/category').then((response) => {
        this.category = response.data.data
      }).catch((err) => {
        console.log(err)
      })
    },
    deleteProduct: function (value) {
      Axios.delete('http://localhost:3000/product/' + value).then((response) => {
        console.log(response)
      }).catch((err) => {
        console.log(err)
      })
    },
    updateProduct: function (value) {
      Axios.put('http://localhost:3000/product/' + value, this.newData).then((response) => {
        console.log(response)
        this.getAllProduct()
      }).catch((err) => {
        console.log(err)
      })
    }
  },
  mounted () {
    this.getAllProduct()
    this.getAllCategory()
  }
}
</script>
<style scoped>
    .content{
      position: fixed;
      margin-top: 70px;
      height: calc(100% - 70px);
      width: calc(100% - 80px);
      margin-left: 80px;
      margin-right: 50px;
      padding: 20px 20px 0 20px;
      overflow: auto;
      background: #E5E5E5;
  }
</style>
