<template>
  <div class="history">
    <Navbar title="History" type="1"/>
    <div class="content">
      <div class="squares row mb-4 ml-4">
                <div class="first-square col-lg-4 col-md-6 col-sm-12">
                    <div>
                        <br><p>Today's Income</p>
                        <h3>Rp. 1.000.000</h3>
                        <p>+2% Yesterday</p><br>
                        </div>
                </div>
                <div class="second-square col-lg-4 col-md-6 col-sm-12">
                    <div>
                        <br><p>Orders</p>
                        <h3>3.270</h3>
                        <p>+5% Last Week</p><br>
                        </div>
                </div>
                <div class="third-square col-lg-4 col-md-12 col-sm-12">
                    <div>
                        <br><p>This Year's Income</p>
                        <h3>Rp. 100.000.000.000</h3>
                        <p>+10% Last Year</p><br>
                        </div>
                </div>
            </div>
            <div class="chart-line">
              <h3 style="padding: 20px;">Revenue</h3>
            <Chart/>
            </div>
      <table class="table table-light mt-4" style="box-shadow: 6px 6px 2px rgba(0,0,0,0.4); border-radius: 10px;">
              <thead>
                  <tr>
                      <th><h2>Recent Order</h2></th>
                  </tr>
              </thead>
              <tbody>
                  <tr>
                      <td>INVOICE</td>
                      <td>CASHIER</td>
                      <td>DATE</td>
                      <td>ORDERS</td>
                      <td>AMOUNT</td>
                      <td>MORE OPTION</td>
                  </tr>
                  <tr v-for="(item, index) in historyList" :key="index">
                      <th>#{{item.invoice}}</th>
                      <th>{{item.cashier}}</th>
                      <th>{{item.date}}</th>
                      <th>{{item.orders}}</th>
                      <th>{{item.amount}}</th>
                      <th><button class="btn btn-success" @click="getMoreInfo(item.invoice); $bvModal.show('modal-info')">Info</button><button class="btn btn-danger" v-on:click="deleteItem(item.invoice)">Delete</button></th>
                  </tr>
              </tbody>
          </table>
    </div>
    <!-- Modal info -->
      <b-modal id="modal-info" hide-footer>
        <template #modal-header>
          More information
        </template>
        <form v-for="(item, index) in result" :key="index">
          <div class="form-group row">
                <label class="col-sm-6 col-form-label">Invoice : {{item.invoice}}</label>
                <div class="col-sm-6">
                  <input type="text" class="form-control" v-model="newData.Invoice">
                </div>
              </div>
              <div class="form-group row">
                <label class="col-sm-6 col-form-label">Cashier: {{item.cashier}}</label>
                <div class="col-sm-6">
                  <input type="text" class="form-control" v-model="newData.cashier">
                </div>
              </div>
              <div class="form-group row">
                <label class="col-sm-6 col-form-label" >Orders: {{item.orders}}</label>
                <div class="col-sm-6">
                  <textarea class="form-control" v-model="newData.orders"></textarea>
                </div>
              </div>
              <div class="form-group row">
                <label class="col-sm-6 col-form-label" >Amount: {{item.amount}}</label>
                <div class="col-sm-6">
                  <input type="text" class="form-control" v-model="newData.amount">
                </div>
              </div>
        <b-button class="btn btn-success" block @click="updateHistory(item.invoice)">Update</b-button>
        <b-button class="btn btn-danger" block @click="$bvModal.hide('modal-info')">Cancel</b-button>
        </form>
      </b-modal>
    <!-- End of modal info -->
  </div>
</template>
<script>
import Navbar from '../components/Navbar'
import Axios from 'axios'
import Chart from '../components/Chart'
export default {
  data () {
    return {
      historyList: [],
      result: [],
      newData: {
        invoice: '',
        cashier: '',
        orders: '',
        amount: ''
      }
    }
  },
  components: {
    Navbar,
    Chart
  },
  methods: {
    getAllHistory: function () {
      Axios.get('http://localhost:3000/history').then((response) => {
        this.historyList = response.data.data
      }).catch((error) => {
        console.log(error)
      })
    },
    deleteItem: function (invoice) {
      Axios.delete('http://localhost:3000/history/' + invoice).then((response) => {
        console.log(invoice)
      }).catch((err) => {
        console.log(err)
      })
    },
    getMoreInfo: function (invoice) {
      Axios.get('http://localhost:3000/detailhistory/' + invoice).then((response) => {
        this.result = response.data
      }).catch((err) => {
        console.log(err)
      })
    },
    updateHistory: function (invoice) {
      Axios.put('http://localhost:3000/history/' + invoice, this.newData).then((response) => {
        console.log(response)
        this.$bvModal.hide('modal-info')
      }).cathc((error) => {
        console.log(error)
      })
    }
  },
  mounted () {
    this.getAllHistory()
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
      padding: 30px;
      overflow: auto;
      background: #E5E5E5;
  }
  .table{
    overflow: auto;
  }
  .first-square{
    background: url('../assets/Rectangle1.png');
    background-repeat: no-repeat;
  }
  .second-square{
    background: url('../assets/Rectangle3.png');
    background-repeat: no-repeat;
  }
  .third-square{
    background: url('../assets/Rectangle2.png');
    background-repeat: no-repeat;
  }
  .chart-line{
    background: #fff;
    border-radius: 10px;
    box-shadow: 6px 6px 2px rgba(0,0,0,0.4);
  }
</style>
