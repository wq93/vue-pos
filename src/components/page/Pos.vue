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
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small>
              <strong>{{totalCount}}</strong> &nbsp;&nbsp;&nbsp;&nbsp;
              <small>总计：</small>
              <strong>{{totalMoney}}</strong> 元
            </div>
            <div class="div-btn">
              <div class="order-btn">
                <el-button type="warning" size="small" @click="checkout">挂单</el-button>
                <el-button type="danger" size="small" @click="delAllGoods">清空</el-button>
                <el-button type="success" size="small" @click="checkout">结账</el-button>
              </div>
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
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="item in oftenGoods" @click="addOrderList(item)">
                <span>{{item.goodsName}}</span>
                <span class="o-price">￥{{item.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <ul class='cookList'>
                <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                  <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%">
                  </span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class='cookList'>
                <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                  <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%">
                  </span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                  <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%">
                  </span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                  <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%">
                  </span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>

          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import $ from 'jquery'
  import axios from 'axios'
  import storage from 'good-storage'

  export default {
    data() {
      return {
        tableData: [],
        oftenGoods: [],
        type0Goods: [],
        type1Goods: [],
        type2Goods: [],
        type3Goods: [],
        totalMoney: 0, // 订单总价格
        totalCount: 0  // 订单商品总数量
      }
    },
    mounted() {
      let tableData = storage.get('tableData')
      this.tableData = tableData
      this.getAllMoney()
      let orderHeight = document.body.clientHeight
      $('#order-list').height(`${orderHeight}px`)
    },
    created() {
      this._getOftenGoods()
      this._getTypeGoods()
    },
    methods: {
      _getOftenGoods() {
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
          .then(response => {
            this.oftenGoods = response.data
          })
          .catch(error => {
            alert('网络错误，不能访问' + error)
          })
      },
      _getTypeGoods() {
        // 读取分类商品列表
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(response => {
            this.type0Goods = response.data[0]
            this.type1Goods = response.data[1]
            this.type2Goods = response.data[2]
            this.type3Goods = response.data[3]
          })
          .catch(error => {
            alert('网络错误，不能访问' + error)
          })
      },
      addOrderList(goods) {
        let isHave = false
        // 判断是否这个商品已经存在于订单列表
        $.each(this.tableData, (index, item) => {
          if (item.goodsId === goods.goodsId) {
            isHave = true // 存在
          }
        })
        // 根据isHave的值判断订单列表中是否已经有此商品
        if (isHave) {
          let arr = this.tableData.filter(o => o.goodsId === goods.goodsId)
          arr[0].count++
          storage.set('tableData', this.tableData)
        } else {
          let newGoods = {
            goodsId: goods.goodsId,
            goodsName: goods.goodsName,
            price: goods.price,
            count: 1
          }
          this.tableData.push(newGoods)
        }
        this.getAllMoney()
      },
      delSingleGoods(goods) {
        this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId)
        this.getAllMoney()
      },
      // 汇总数量和金额
      getAllMoney() {
        this.totalCount = 0
        this.totalMoney = 0
        if (this.tableData) {
          this.tableData.forEach((element) => {
            this.totalCount += element.count
            this.totalMoney = this.totalMoney + (element.price * element.count)
          })
        }
      },
      delAllGoods() {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
      },
      checkout() {
        if (this.totalCount !== 0) {
          this.tableData = []
          this.totalCount = 0
          this.totalMoney = 0
          this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
          })
        } else {
          this.$message.error('不能空结。老板了解你急切的心情！')
        }
      }
    },
    watch: {
      tableData(newList) {
        console.log('变化了')
        storage.set('tableData', newList)
      }
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

  .order-btn {
    margin-top: 10px;
    text-align: center;
  }

  .title {
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
  }

  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
    cursor: pointer;
  }

  .goods-type {
    clear: both;
  }

  .o-price {
    color: #58B7FF;
  }

  .often-goods-list {
    border-bottom: 1px solid #C0CCDA;
    height: auto;
    overflow: hidden;
    padding-bottom: 10px;
    background-color: #F9FAFC;
  }

  .cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
  }

  .cookList li span {

    display: block;
    float: left;
  }

  .foodImg {
    width: 40%;
  }

  .foodName {
    font-size: 18px;
    padding-left: 10px;
    color: brown;
  }

  .foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
  }

  .totalDiv {
    height: auto;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
  }
</style>
