<template>
  <div class="tableContainer">
    <table class="table table-striped dataTable" v-if="headers.length && tableData.length > 0 ">
      <thead>
        <tr class="tableHead">
          <th scope="col" class="font-weight-normal" v-for="(el, ndx) in headers" :key="ndx"> {{el.text}} </th>
        </tr>
      </thead>
      <tbody v-if="tableData.length > 0">
        <tr class="noBorder font-weight-bold" v-for="(el, index) in tableData" :key="index + 'data'">
          <td 
            v-for="(h, x) in headers" 
            :key="x"
            @click="h.key.toLowerCase() === 'id' || h.key.toLowerCase() === 'order_id' ? onOrderIdClick(index, el[h.key]) : () => {}"
            :style="h.key.toLowerCase() === 'id' || h.key.toLowerCase() === 'order_id' ? 'color: #0064B1; cursor: pointer' : ''"
          > 
            {{
              (el[h.key] === null || el[h.key] === "" || el[h.key] === undefined) ?
                h.key === 'returned' ? 
                  el['crockery_status'] === 'Complete' ?
                    'Yes'
                  :
                    'No'
                :
                  h.key === 'order_total' ?
                    el['order_details'] !== undefined ?
                      global.currency[0].text + ' ' + el['order_details'][h.key]
                    :
                      ''
                  :
                    '----'
              :
                (h.key.toLowerCase() === 'created_on_utc' || h.key.toLowerCase() === 'created_date_utc') 
                ? 
                  returnDate(el) 
                : 
                  (h.key === 'order_total' ?
                    global.currency[0].text + ' ' + el[h.key] 
                  : 
                    el[h.key])
            }} 
          </td>
        </tr>
      </tbody>
    </table>
    <div v-else class="notFoundContainer">
      <div class="notFound text-center">
        <img :src="require('src/assets/img/logo_white.png')" style="width: 20%; height: auto;"/>
        <div class="mt-2">
          <b class="font-weight-normal"> 404 | No data yet.</b>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import global from 'src/helpers/global'
export default {
  props: ['tableData', 'headers', 'orderClicked'],
  data() {
    return {
      viewType: 'normal_view', // normal_view, weekly_view, view_all,
      global: global
    }
  },
  watch: {
    headers: function(_new, old) {
      return _new
    },
    tableData: function(_new, old) {
      return _new
    }
  },
  mounted() {},
  updated() {},
  methods: {
    onOrderIdClick(index, orderId) {
      this.orderClicked(index, orderId)
    },
    returnDate(el) {
      let d = new Date(el.created_on_utc ? el.created_on_utc : el.created_date_utc)
      let dd = d.getDate()
      let mm = d.getMonth() + 1
      let yy = d.getFullYear()
      if(String(dd).length < 2) {
        dd = '0' + dd
      }
      if(String(mm).length < 2) {
        mm = '0' + mm
      }
      return yy + '-' + mm + '-' + dd
    }
  }
}
</script>
<style scoped>
.notFoundContainer {
  min-height: 80vh;
}
.notFound {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  display: inline-block;
}
.dataTable {
  color: #4D4E4F;
}
.noBorder td{
  border: none !important;
}
.tableHead th{
  border-top: 1px solid #707070 !important;
  border-bottom: 1px solid #707070 !important;
}
.tableContainer {
  height: 80vh;
  border-top: 1px solid #707070;
  border-bottom: 1px solid #707070;
  border-left: 1px solid #707070;
  border-right: 1px solid #707070;
  overflow-y: scroll;
}
.tableContainer::-webkit-scrollbar-track
{
	/* -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); */
	background-color: #F5F5F5;
}
.tableContainer::-webkit-scrollbar
{
  width: 5px;
	background-color: #F5F5F5;
}
.tableContainer::-webkit-scrollbar-thumb
{
	background-color: #707070;
	border: 0px;
  border-radius: 10px;
}
</style>
