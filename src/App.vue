<template>
  <div id="app">
    <!-- Main -->
    <div class="main-content">
      <div class="sidebar">
        <ul>
          <li><router-link to="/"><img src="../src/assets/Icon/fork.png" alt=""></router-link></li>
          <li><router-link to="/history"><img src="../src/assets/Icon/clipboard.png" alt=""></router-link></li>
          <li><router-link to="" v-b-modal.modal-1><img src="../src/assets/Icon/add.png" alt=""></router-link></li>
        </ul>
      </div>
        <router-view/>
    </div>
    <!-- End of Main -->
    <!-- Modal -->
            <b-modal id="modal-1" title="Add Items">
              <form>
              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Name</label>
                <div class="col-sm-10">
                  <input type="text" class="form-control" v-model="addMenu.product_name">
                </div>
              </div>
              <div class="form-group row">
                <label class="col-sm-2 col-form-label" >Image</label>
                <div class="col-sm-10">
                  <input type="text" class="form-control" v-model="addMenu.image">
                </div>
              </div>
            </form>
           <form>
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Price</label>
              <div class="col-sm-6">
                <input type="text" class="form-control" v-model="addMenu.price">
              </div>
            </div>
            <div class="form-group row">
              <label for="inputState" class="col-sm-2 col-form-label">Category</label>
              <div class="col-sm-4">
                <select id="inputState" class="form-control" v-model="addMenu.id_category">
                  <option v-for="(item, index) in listCategory" :key="index" :value="item.id">
                    {{item.category_name}}
                  </option>
                </select>
              </div>
            </div>
           </form>
    <template #modal-footer>
        <div class="w-100">
        <b-button type="button" class="btn text-white border-0" style="background-color: #F24F8A;" block @click="$bvModal.hide('modal-1')">Cancel</b-button>
        <b-button type="button" class="btn text-white border-0" style="background-color:#57CAD5;" block @click="sendToMenu">Add</b-button>
        </div>
      </template>
    </b-modal>
    <!-- End of modal -->
  </div>
</template>

<script>
import Axios from 'axios'
export default {
  data () {
    return {
      listCategory: [],
      addMenu: {
        id_category: '',
        product_name: '',
        image: '',
        price: ''
      }
    }
  },
  props: ['title', 'type'],
  methods: {
    sendToMenu: function () {
      Axios.post('http://localhost:3000/product', this.addMenu).then((response) => {
        console.log(response)
        this.$bvModal.hide('modal-1')
      }).catch((error) => {
        console.log(error)
      })
    },
    getAllCategory: function () {
      Axios.get('http://localhost:3000/category').then((response) => {
        this.listCategory = response.data.data
      }).catch((error) => {
        console.log(error)
      })
    }
  },
  mounted () {
    this.getAllCategory()
  }
}
</script>

<style>
  @font-face{
    font-family: 'Cereal';
    src: url('../src/assets/AirbnbCerealBold.ttf') format('truetype');
  }
  * {
    margin: 0;
    padding: 0;
    list-style: none;
    box-sizing: border-box;
    text-decoration: none;
    font-family: 'Cereal';
  }
  .main-content .sidebar{
    position: fixed;
    top: 70px;
    left: 0;
    width: 80px;
    height: 100%;
  }
  .main-content .sidebar ul li img{
    padding: 10px 10px 10px 15px;
    justify-content: center;
    margin-top: 5px;
  }
  .main-content .sidebar ul li:hover{
    background: #E5E5E5;
  }
</style>
