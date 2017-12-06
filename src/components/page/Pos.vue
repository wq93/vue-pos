<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' id="order-list" class="pos-order">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border show-summary style="width: 100%">

              <el-table-column prop="goodsName" label="商品"></el-table-column>
              <el-table-column prop="count" label="量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small">删除</el-button>
                  <el-button type="text" size="small">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="div-btn">
              <el-button type="warning" size="small">挂单</el-button>
              <el-button type="danger" size="small">删除</el-button>
              <el-button type="success" size="small">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <!--商品展示-->
      <el-col :span="17">
        我是产品栏
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import $ from 'jquery'

  export default {
    data() {
      return {
        tableData: [{
          goodsName: '可口可乐',
          price: 8,
          count: 1
        }, {
          goodsName: '香辣鸡腿堡',
          price: 15,
          count: 1
        }, {
          goodsName: '爱心薯条',
          price: 8,
          count: 1
        }, {
          goodsName: '甜筒',
          price: 8,
          count: 1
        }]
      }
    },
    mounted() {
      let orderHeight = document.body.clientHeight
      $('#order-list').height(`${orderHeight}px`)
    }
  }
</script>
<style scoped>
  .pos {
    font-size: 12px;
  }

  .pos-order {

    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
  }
</style>
