<template>
  <div class="htmleaf-container">
    <header class="htmleaf-header">
      <h1>jquery九宫格抽奖转盘特效</h1>
    </header>
    <div id="lottery">
      <table border="0" cellpadding="0" cellspacing="0">
        <tr>
          <td class="lottery-unit lottery-unit-0"><img src="../assets/1.jpg">
            <div class="mask"></div>
          </td>
          <td class="lottery-unit lottery-unit-1"><img src="../assets/2.jpg">
            <div class="mask"></div>
          </td>
          <td class="lottery-unit lottery-unit-2"><img src="../assets/3.jpg">
            <div class="mask"></div>
          </td>
        </tr>
        <tr>
          <td class="lottery-unit lottery-unit-7"><img src="../assets/4.jpg">
            <div class="mask"></div>
          </td>
          <td>
            <a href="#"></a>
          </td>
          <td class="lottery-unit lottery-unit-3"><img src="../assets/5.jpg">
            <div class="mask"></div>
          </td>
        </tr>
        <tr>
          <td class="lottery-unit lottery-unit-6"><img src="../assets/6.jpg">
            <div class="mask"></div>
          </td>
          <td class="lottery-unit lottery-unit-5"><img src="../assets/7.jpg">
            <div class="mask"></div>
          </td>
          <td class="lottery-unit lottery-unit-4"><img src="../assets/8.jpg">
            <div class="mask"></div>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data() {
    return {
      index: -1,    //当前转动到哪个位置，起点位置
      count: 0,    //总共有多少个位置
      timer: 0,    //setTimeout的ID，用clearTimeout清除
      speed: 20,    //初始转动速度
      times: 0,    //转动次数
      cycle: 50,    //转动基本次数：即至少需要转动多少次再进入抽奖环节
      prize: -1,
      click: false,
    }
  },
  methods: {
    init: function(id) {
      if ($("#" + id).find(".lottery-unit").length > 0) {
        var $lottery = $("#" + id);

        var $units = $lottery.find(".lottery-unit");
        this.obj = $lottery;
        this.count = $units.length;
        $lottery.find(".lottery-unit-" + this.index).addClass("active");
      };
    },
    roll: function() {
      var index = this.index;
      var count = this.count;
      var lottery = this.obj;
      $(lottery).find(".lottery-unit-" + index).removeClass("active");
      index += 1;
      if (index > count - 1) {
        index = 0;
      };
      $(lottery).find(".lottery-unit-" + index).addClass("active");
      this.index = index;
      return false;
    },
    stop: function(index) {
      this.prize = index;
      return false;
    },
    initRoll() {
      this.times += 1;
      this.roll();//转动过程调用的是lottery的roll方法，这里是第一次调用初始化
      if (this.times > this.cycle + 10 && this.prize == this.index) {
        alert("aa")
        clearTimeout(this.timer);
        this.prize = -1;
        this.times = 0;
        this.click = false;
        this.LotteryTimes -= 1;
        if (this.LotteryTimes < 0) {
          this.LotteryTimes = 3;
        }
      } else {
        if (this.times < this.cycle) {
          this.speed -= 10;
        } else if (this.times == this.cycle) {
          var index = Math.random() * (this.count) | 0;//中奖物品通过一个随机数生成
          this.prize = index;
        } else {
          if (this.times > this.cycle + 10 && ((this.prize == 0 && this.index == 7) || this.prize == this.index + 1)) {
            this.speed += 110;
          } else {
            this.speed += 20;
          }
        }
        if (this.speed < 40) {
          this.speed = 40;
        };
        // console.log(this.times+'^^^^^^'+this.speed+'^^^^^^^'+this.prize);
        // console.log(this.initRoll)
        // console.log(this.initRoll())

        this.timer = setTimeout(this.initRoll, this.speed);//循环调用

      }
      return false;
    },
    startInit() {
      this.init('lottery');
      var that = this;
      $("#lottery a").click(function() {
        if (that.click) {//click控制一次抽奖过程中不能重复点击抽奖按钮，后面的点击不响应
          return false;
        } else {
          that.speed = 100;
          that.initRoll();    //转圈过程不响应click事件，会将click置为false
          that.click = true; //一次抽奖完成后，设置click为true，可继续抽奖
          return false;
        }
      });
    }
  },
  mounted() {
    this.startInit();

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#lottery {
  width: 570px;
  height: 510px;
  margin: 0px auto;
  border: 4px solid #ba1809;
}

#lottery table {
  background-color: yellow;
}

#lottery table td {
  position: relative;
  width: 190px;
  height: 170px;
  text-align: center;
  color: #333;
  font-index: -999
}

#lottery table td img {
  display: block;
  width: 190px;
  height: 170px;
}

#lottery table td a {
  width: 190px;
  height: 170px;
  display: block;
  text-decoration: none;
  background: url("../assets/9.jpg") no-repeat top center;
}

#lottery table td a:hover {
  background-image: url("../assets/11.jpg");
}

#lottery table td.active .mask {
  display: block;
}

.mask {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  background-color: rgba(252, 211, 4, 0.5);
  display: none;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
