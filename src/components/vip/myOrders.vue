<template>
  <div class="el-row">
    <div class="el-col el-col-24">
      <div class="comp">
        <div class="tmpl routeanimate">
          <div class="section">
            <div class="location">
              <span>当前位置：</span>
              <a href="/index.html">首页</a> &gt;
              <a href="#/site/member/center" class="">会员中心</a>&gt;
              <a href="#/site/member/orderlist" class="router-link-exact-active ">我的订单</a>
            </div>
          </div>
          <div class="section clearfix">
            <div class="left-260">
              <div class="bg-wrap">
                <div class="avatar-box">
                  <a href="/user/center/avatar.html" class="img-box">
                    <i class="iconfont icon-user-full"></i>
                  </a>
                  <h3>

                    ivanyb

                  </h3>
                  <p>
                    <b>注册会员</b>
                  </p>
                </div>
                <div class="center-nav">
                  <ul>
                    <li>
                      <h2>
                        <i class="iconfont icon-order"></i>
                        <span>订单管理</span>
                      </h2>
                      <div class="list">
                        <p>
                          <router-link to="/site/myOrders" href="javascript:void(0)" class="router-link-exact-active ">
                            <i class="iconfont icon-arrow-right"></i>交易订单</router-link>
                        </p>
                      </div>
                    </li>
                    <li>
                      <h2>
                        <i class="iconfont icon-user"></i>
                        <span>账户管理</span>
                      </h2>
                      <div class="list">
                        <p>
                          <a href="javascript:void(0)" class="">
                            <i class="iconfont icon-arrow-right"></i>账户资料</a>
                        </p>
                        <p>
                          <a href="javascript:void(0)" class="">
                            <i clrouter-linkss="iconfont icon-router-linkrrow-right"></i>头像设置</a>
                        </p>
                        <p>
                          <a href="javascript:void(0)" class="">
                            <i class="iconfont icon-arrow-right"></i>修改密码</a>
                        </p>
                        <p>
                          <a href="javascript:void(0)">
                            <i class="iconfont icon-arrow-right"></i>退出登录</a>
                        </p>
                      </div>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
            <div class="right-auto">
              <div class="bg-wrap" style="min-height: 765px;">
                <div class="sub-tit">
                  <ul>
                    <li class="selected">
                      <a href="/user/order-list.html">交易订单</a>
                    </li>
                  </ul>
                </div>
                <div class="table-wrap">
                  <table width="100%" border="0" cellspacing="0" cellpadding="0" class="ftable">
                    <tbody>
                      <tr>
                        <th width="16%" align="left">订单号</th>
                        <th width="10%">姓名</th>
                        <th width="12%">订单金额</th>
                        <th width="11%">下单时间</th>
                        <th width="10%">状态</th>
                        <th width="12%">操作</th>
                      </tr>
                      <tr v-for="item in orderList" :key="item.id">
                        <td>{{item.order_no}}</td>
                        <td align="left">{{item.accept_name}}</td>
                        <td align="left">
                          <strong style="color: red;">￥{{item.order_amount}}</strong>
                          <br> 在线支付
                        </td>
                        <td align="left">{{item.add_time | moment("YYYY-MM-DD HH:mm:ss")}}</td>
                        <td align="left">
                          {{item.statusName}}
                        </td>
                        <td align="left">
                          <router-link :to="'/site/orderInfo/'+item.id" class="">查看订单</router-link>
                          <br>
                          <router-link v-if="item.status == 1" :to="'/site/payOrder/'+item.id" class="">|去付款</router-link>
                          <br>
                          <a href="javascript:void(0)">|取消</a>
                          <br>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <div class="page-foot">
                    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pageIndex" :page-sizes="[10, 20, 30, 50]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper" :total="totalCount">
                    </el-pagination>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      orderList: [],
      totalCount: 0,
      pageIndex: 1, //当前页面索引
      pageSize: 10 //每页页容量
    };
  },
  created() {
    this.getOrderListData();
  },
  beforeUpdate() {
    // console.log("----myOrders---- beforeUpdate----");
  },
  updated() {
    // console.log("----myOrders---- updated----");
  },
  methods: {
    //分页获取订单数据
    getOrderListData() {
      const url = `site/validate/order/userorderlist?pageIndex=${
        this.pageIndex
      }&pageSize=${this.pageSize}`;

      this.$axios.get(url).then(response => {
        this.totalCount = response.data.totalcount;
        this.orderList = response.data.message;
      });
    },
    //分页相关
    handleSizeChange(pageSize) {
      this.pageSize = pageSize;
      this.pageIndex = 1;
      this.getOrderListData();
    },
    handleCurrentChange(pageIndex) {
      this.pageIndex = pageIndex;
      this.getOrderListData();
    }
  }
};
</script>