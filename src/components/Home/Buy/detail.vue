<template>
    <div class="mui-content">
        <div class="detail">
            <swipe :imgUrl='url'></swipe>
        </div>
        <div class="sell">
            <h4>{{goods.title}} </h4>
            <div class="price">
                市场价：<s>￥{{goods.sell_price}} </s> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 销售价：<span>￥{{goods.market_price}}</span>
            </div>
            <div class="num">
                购买数量：<number @numberchange="numberchanged" :stock="goods.stock_quantity"></number>
                <div v-if="false"  class="ball"></div>
            </div>
            <div class="button">
                <button class="mui-btn mui-btn-primary">立刻购买</button>
                <button @click='addcart' class="mui-btn mui-btn-danger">加入购物车</button>
            </div>
        </div>

        <div class="param">
            <h5>商品参数</h5>
            <div class="info">
                <p>商品编号：{{goods.goods_no}}</p>
                <p>库存情况：{{goods.stock_quantity}}件</p>
                <p>上架时间：{{goods.add_time|fmtdate('YYYY-MM-DD')}}</p>
            </div>
        </div>
        
        <div class="footer">
            <a class="mui-btn mui-btn-primary mui-btn-outlined">图文介绍</a>
            <a class="mui-btn mui-btn-danger mui-btn-outlined">商品评论</a>
        </div>


    </div>
</template>

<script>
import swipe from "../../Common/swipe.vue";
import number from "../../Common/number.vue";
import vueObj from "../../../config/communication"
//导出组件
export default {
  components: {
    swipe,
    number
  },
  props: ["id"],
  data() {
    return {
      url: "getthumimages/" + this.id,
      goods: [],
      count: 1
    };
  },
  created() {
    this.getgoods();
  },
  methods: {
    getgoods() {
       
      this.$http
        .get("goods/getinfo/" + this.id)
        .then(res => {
          if (res.status === 200 && res.data.status === 0) {
            if (res.data.message.length > 0) {
              this.goods = res.data.message[0];
              console.log(this.goods);
            }
          } else {
            console.log("服务器内部错误");
          }
        })
        .catch(err => {
          console.error(err);
        });
    },
    // number组件中的数字发生变化后会调用
    numberchanged(count) {
      // console.log(count);
      this.count = count;
       console.log(this.count);
    },
    addcart(){
        vueObj.$emit('updateBadge',this.count);
    }
  }
};
</script>

<style scoped>
.num {
  position: relative;
}

.ball {
  left: 115px;
  top: 3px;
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 10px;
  background-color: red;
  transition: all 0.5s linear;
  z-index: 1000;
}

.mui-content {
  background-color: #fff;
}

.top,
.sell,
.param,
.footer {
  border: 1px solid rgba(92, 92, 92, 0.7);
  border-radius: 5px;
  margin: 5px;
  padding: 10px;
}

.sell h4 {
  color: dodgerblue;
  padding-bottom: 10px;
  border-bottom: 1px solid rgba(92, 92, 92, 0.5);
}

.price,
.num,
.button {
  margin: 10px;
  font-size: 15px;
}

.price span {
  font-size: 18px;
  color: red;
}

.param h5 {
  font-weight: bold;
  padding-bottom: 10px;
  border-bottom: 1px solid rgba(92, 92, 92, 0.5);
}

.info {
  margin-top: 10px;
  margin-left: 20px;
}

.info p {
  margin: 0;
}

.footer .mui-btn {
  width: 100%;
}

.footer .mui-btn-danger {
  margin-top: 10px;
}
</style>
