<style scoped>
  @import "../assets/css/dashboard.css";
</style>
<template>
  <section class="dashboard-box">
    <div class="dashboard-box-inner">
      <div class="notice map-box-out">
        <header>实时公告</header>
        <div class="noticediv f-scroll-lt" id="notice-box" @mouseover="noticeMouseEvent(false)" @mouseleave="noticeMouseEvent(true)" :style="{height:noticeUlHeight}">
          <ul class="m-notice-ul" id="notice-ul">
            <li class="clr" v-for="msg in notice">
              <div class="fl msg-type">
                <span class="u-icon-chongzhi" v-show="msg.type==3"></span>
                <span class="u-icon-jili" v-show="msg.type==4"></span>
                <span class="u-icon-kaika" v-show="msg.type==5"></span>
                <span class="u-icon-jifen" v-show="msg.type==2"></span>
                <span class="u-icon-icon_yewu" v-show="msg.type==1"></span>
                <span class="u-icon-liuliang" v-show="msg.type==9"></span>
                <span class="u-icon-zhuce" v-show="msg.type==6"></span>
                <span class="u-icon-jihuo" v-show="msg.type==7"></span>
                <span class="u-icon-huafei" v-show="msg.type==8"></span> 
              </div>
              <div class="msg-content">
                <time>{{ getDateTime(msg.createTime)[5] }}</time>
                <div class="content">{{ msg.message }}</div>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <!--第一屏-->
      <div class="map-box-out make_transist" v-bind:class="{'showscale' : isA, 'hidescale': !isA}">
        <div class="map-box">
          <!--总开卡成功数-->
          <div class="map-box-inner" id="amount"></div>
          <div class="inner-total total-3">（<span>总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(amountCard[0])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）
                      </div>
          <div class="statistics">
            <div class="col">
              <div>远特普号<span class="media-540">（可售）</span></div><b class="fsfantasy">
                <CountUp :start="0"
                        :end="parseInt(phoneTotal[0])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
              <div>远特靓号<span class="media-540">（可售）</span></div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(phoneTotal[1])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b></b>
            </div>
            <div class="col">
              <div>联通普号<span class="media-540">（可售）</span></div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(phoneTotal[2])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b></b>
              <div>联通靓号<span class="media-540">（可售）</span></div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(phoneTotal[3])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b></b>
            </div>
          </div>
        </div>
        <!--签约/激活商户-->
        <div class="map-box">
          <div class="map-box-inner" id="merchant"></div>
          <div class="inner-total total-3">（<span>签约总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(merchantTotal[0])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
                        <span>激活总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(merchantTotal[1])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）</div>
          <div class="statistics">
            <div class="col">
              <div>游客</div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(userTotal[0])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
              <div>远特商户/工号数</div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(userTotal[1])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp>/
                      <CountUp :start="0"
                        :end="parseInt(userTotal[4])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
            </div>
            <div class="col">
              <div>联通商户/工号数</div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(userTotal[2])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp>/
                      <CountUp :start="0"
                        :end="parseInt(userTotal[5])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
              <div>设备数</div><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(userTotal[3])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>
            </div>
          </div>
        </div>
      </div>
      <!--第二屏-->
      <div class="map-box-out make_transist" v-bind:class="{'showscale' : !isA, 'hidescale': isA}">
        <div class="map-box">
          <!--开卡尝试-->
          <div class="map-box-inner" id="cardCreate"></div>
          <div class="inner-total total-1">（<span>历史总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(secondViewTotal[0])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）</div>
          <!--开卡成功数-->
          <div class="map-box-inner" style="flex:1.2" id="openCardSuccess"></div>
          <div class="inner-total total-2">（<span>历史总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(secondViewTotal[1])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）</div>
        </div>
        <div class="map-box">
          <!--在线用户-->
          <div class="map-box-inner" id="onlineUser"></div>
          <div class="inner-total total-1">（<span>历史总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(secondViewTotal[2])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）</div>
          <!--充值金额-->
          <div class="map-box-inner" style="flex:1.2" id="rechargeMoney"></div>
          <div class="inner-total total-2">（<span>历史总数：</span><b class="fsfantasy"><CountUp :start="0"
                        :end="parseInt(secondViewTotal[3])"
                        :decimals="0"
                        :duration="3"
                        :options="countOptions"
                        :callback="callback"></CountUp></b>）</div>
        </div>
      </div>
    </div>
    <span  v-on:click="toogle" class="switch u-icon-shiftEchart"></span>
  </section>
</template>
<script type="text/javascript">
import { getDateTime,getUnixTime } from "../config/utils.js";
import CountUp from 'vue-countup-v2';
  export default {
    name: "dashboard",
    data() {
      return {
        isA: true,
        countOptions:{//countUp options
          useEasing: true,
          useGrouping: true,
          separator: ',',
          decimal: '.',
          prefix: '',
          suffix: ''
        },
        notice:[],//公共数据列表
        noticeUlHeight:'',//公共栏高
        noticelatestMaxid:0,//最近获取消息的时间
        TimerNotice:[],//消息定时器
        TimerLine:[],
        off:{
          cardTry_index:0,//开卡尝试legend selected index
          onlineUser_index:0,//在线用户legend selected index
          recharge_index:0,//充值legend selected index
          cardSuccess_index:0,//开卡成功legend selected index
        },
        phoneTotal:[0,0,0,0],//0,远特普号;1,远特靓号;2,联通普号;3,联通靓号;
        userTotal:[0,0,0,0,0,0],//0,游客;1,远特商户数;2,联通商户数;3,设备数;4,远特工号数;5,联通工号数;
        secondViewTotal:[0,0,0,0],//0,开卡尝试;1,在线商户;2,在线用户;3,充值金额;
        merchantTotal:[0,0],//0,签约商户总数;1,激活商户总数;
        amountCard:[0],//总开卡成功数
      };
    },
    components: {
      CountUp
    },
    created:function(){
      const vm=this,change=function(){
        vm.noticeUlHeight=document.documentElement.clientHeight-80+'px';
      };
      change();
      window.onresize=function(){
        change();
      };


    },
    beforeDestroy:function(){
      this.noticeMouseEvent();
      this.intervalGetLine();
    },
    mounted() {
      //   ajax请求获取数据统计
      var vm=this;
      
      vm.getnotice();

      setTimeout(function(){//第一屏
        vm.getamount();
        vm.getcreatecardAmount();
        vm.getmerchant();
      },500);
      

      setTimeout(function(){//第二屏
        vm.getcardCreate();
        vm.getonlineUser();
        vm.getrechargeMoney();
        vm.getOpenCardSuccess();
      },2500);
      

      vm.noticeMouseEvent(true);
      vm.intervalGetLine(true);
    },
    methods: {
      callback: function(ins) {
        // ins.update(ins.endVal + 100);
      },
      intervalGetLine(off){//5分钟轮循获取折线图数据
        const vm=this;
        if(off){ 
          let timer=setInterval(function(){
              vm.getamount();
              vm.getcreatecardAmount();
              vm.getmerchant();
              
              vm.getcardCreate();
              vm.getonlineUser();
              vm.getrechargeMoney();
              vm.getOpenCardSuccess();
          },1000*60*5);
          vm.TimerLine.push(timer)
        }else{
          for(let i=0;i<vm.TimerLine.length;i++){
            clearInterval(vm.TimerLine[i]);
          }
        }
      },
      toogle:function(){//切屏
        this.isA = !this.isA;
      },
      noticeMouseEvent(off){//轮循获取公共数据数据
        const vm=this;
        if(off){ 
          let timer=setInterval(function(){
              vm.getnotice();
          },2000);
          vm.TimerNotice.push(timer)
        }else{
          for(let i=0;i<vm.TimerNotice.length;i++){
            clearInterval(vm.TimerNotice[i]);
          }
        }
      },
      getLatelyTime(now,type,num){//获取最近日期
        let dateArr=[];
        for(let i=(num-1);i>=0;i--){
          if(type=='hour'){
            if(i!=11){
              let sm=getDateTime(now-1000*60*60*i)[7];
              dateArr.push(sm.split(":")[0]+":00");
              if(i==0){
                dateArr.push(getDateTime(now-1000*60*60*i)[7]);
              }
            }
            
          };
          if(type=='day')dateArr.push(getDateTime(now-1000*60*60*24*i)[4]);
          if(type=='month')dateArr.push(getDateTime(now-1000*60*60*24*30*i)[1]);
        }
        return dateArr
      },
      getnotice(){//获取公告
        var vm=this;
        vm.AJAX('w/statistics/realtimenotice',{"maxId":vm.noticelatestMaxid},function(data){
          if(data.code==200){
            data.data.list.reverse();
            for(let i=0;i<data.data.list.length;i++){
              vm.notice.push(data.data.list[i]);
            }

            setTimeout(function(){
              vm.noticeScroll();
            },30)
            
            if(data.data.list[0]){
              vm.noticelatestMaxid=data.data.list[0].maxId;
            }
          }
        },true);
      },
      noticeScroll(){//消息滚动执行
        var vm=this,noticeBox=document.getElementById("notice-box");
        if(noticeBox)noticeBox.scrollTop=noticeBox.scrollHeight;
        if(vm.notice.length>100){
          vm.notice=[];
        }
      },
      getamount(){//获取第一屏右边总数
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":1},function(data){
          if(data.code==200){
            vm.phoneTotal = [
              data.data.YT_sellbleGeneralNum,
              data.data.YT_sellblePrettyNum,
              data.data.LT_sellbleGeneralNum,
              data.data.LT_sellblePrettyNum
            ];
            vm.userTotal = [
              data.data.visitor,
              data.data.YT_merch,
              data.data.LT_merch,
              data.data.device,
              data.data.YT_userNum,
              data.data.LT_userNum
            ];
          }
        },true);
      },
      getcreatecardAmount(){//总开卡成功数
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":2},function(data){
          vm.initMap({
            id: "amount",
            name: "总开卡成功数",
            index:0,
            legend: ["天"],
            category: [vm.getLatelyTime(data.data.lasttime,'day',7)],
            lineData: [data.data.day]
          });
          vm.amountCard[0]=data.data.day[6];
        },true);
      },
      getmerchant(){//获取签约商户数和激活商户
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":3},function(data){
          vm.initMap({
            id: "merchant",
            name: "",
            index:0,
            legend: ["签约商户","激活商户"],
            category: [vm.getLatelyTime(data.data.lasttime,'day',7),vm.getLatelyTime(data.data.lasttime,'day',7)],
            lineData: [data.data.signMerch,data.data.activeMerch]
          });
          vm.merchantTotal=[data.data.totalSign,data.data.totalActive]
        },true);
      },
      getcardCreate(){//开卡尝试数
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":5},function(data){
          vm.secondViewTotal[0] = data.data.total;
          vm.initMap({
            id: "cardCreate",
            name: "开卡尝试",
            legend: ["时", "天", "月"],
            category: [vm.getLatelyTime(data.data.lasttime[0],'hour',12),vm.getLatelyTime(data.data.lasttime[1],'day',7),vm.getLatelyTime(data.data.lasttime[2],'month',6)],
            lineData: [data.data.hour,data.data.day,data.data.month],
            index:vm.off.cardTry_index
          });
        },true);
      },
      getonlineUser(){//用户在线数
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":6},function(data){
          vm.secondViewTotal[2] = data.data.total;
          vm.initMap({
            id: "onlineUser",
            name: "在线用户",
            legend: ["时","天"],
            index:vm.off.onlineUser_index,
            category: [vm.getLatelyTime(data.data.lasttime[0],'hour',12),vm.getLatelyTime(data.data.lasttime[1],'day',7)],
            lineData: [data.data.hour,data.data.day]
          });
        },true);
      },
      getrechargeMoney(){//充值金额
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":7},function(data){
          vm.secondViewTotal[3] = data.data.total;
          vm.initMap({
            id: "rechargeMoney",
            name: "充值金额",
            index:vm.off.recharge_index,
            legend: ["时", "天", "月"],
            category: [vm.getLatelyTime(data.data.lasttime[0],'hour',12),vm.getLatelyTime(data.data.lasttime[1],'day',7),vm.getLatelyTime(data.data.lasttime[2],'month',6)],
            lineData: [data.data.hour,data.data.day,data.data.month]
          });
        },true);
      },
      getOpenCardSuccess(){//开卡成功数
        var vm=this;
        vm.AJAX('w/statistics/homepagedrawline',{"type":4},function(data){
          vm.secondViewTotal[1] = data.data.total;
          vm.initMap({
            id: "openCardSuccess",
            name: "开卡成功数",
            legend: ["时", "天", "月"],
            index:vm.off.cardSuccess_index,
            category: [vm.getLatelyTime(data.data.lasttime[0],'hour',12),vm.getLatelyTime(data.data.lasttime[1],'day',7),vm.getLatelyTime(data.data.lasttime[2],'month',6)],
            lineData: [data.data.hour,data.data.day,data.data.month],
          });
        },true);
      },
      filterNumber(num){//数字格式化
        // num=num.toString();
        // const filter=s=>{
        //   let result = [ ], counter = 0;
        //   s=(s || 0).toString().split('');
        //   for(let i = s.length - 1; i >= 0; i--) {
        //       counter++;
        //       result.unshift(s[i]);
        //       if (!(counter % 3) && i != 0) { result.unshift(','); }
        //   }
        //   return result.join('');
        // }
        // if(num.indexOf(".")>-1){
        //   let arr=num.split(".");
        //   return filter(arr[0])+'.'+arr[1];
        // }else return filter(num)
      },
      initMap(params) {
        var vm = this,series = [],min=[],myChart = '';
        if(document.getElementById(params.id)==null){
          return false;
        }else{
          myChart = echarts.init(document.getElementById(params.id));
          myChart.showLoading();
        for (let i = 0; i < params.legend.length; i++) {
          let option={
            name: params.legend[i],
            type: "line",
            smooth: true,
            showAllSymbol: false,
            symbol: "emptyCircle",
            symbolSize: 5,
            data: params.lineData[i],
            itemStyle: {
              normal: {
                width: 5,
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: "#7E57C2" },
                  { offset: 1, color: "#4b3887" }
                ])
              }
            },
            // areaStyle: {normal: {}},
          }
          if(params.id=="merchant"){
            if(i==1)option.itemStyle.normal.color="#3DD79B";
            option.yAxisIndex=i;
          }
          min[i]=Math.min.apply(null,params.lineData[i]);
          series.push(option);
        }
        for(let j=0;j<min.length;j++){//最小值取整
          if(min[j]>10){
            let num=min[j].toString(),len=num.length,power;
            if(len>=4){
              power=Math.pow(10,(len-2));
            }else{
              power=Math.pow(10,(len-1));
            }
            num=parseInt(num/power)*power;
            min[j]=num;
          };
        }
        //console.log(params.category[0])
        // option
        var option = {
          title: {
            show: true,
            text: params.name,
            textStyle: {
              color: "#555",
              fontSize:"15",
            },
            left:20
          },
          grid:{
            y:50,
          },
          tooltip: {
            show:true,
            trigger: "axis",
            axisPointer: {
              type: "line",
              label: {
                show: true,
                backgroundColor: "#333"
              }
            },
            formatter:params.id=="merchant" ? '{a0}: {c0}<br />{a1}: {c1}' : '{c}'
          },
          legend: {
            data: params.legend,
            selectedMode: params.id=="merchant" ? "multiple" : "single",
            textStyle: {
              color: "#555"
            },
            left:120,
            top:3
          },
          xAxis: {
            type : 'category',
            boundaryGap : false,
            data: params.category[params.index],
            axisLine: {
              lineStyle: {
                color: "#555"
              }
            },
            splitLine:{
              show:false
            },
          },
          yAxis: [{
            splitLine: { show: false },
            axisLine: {
              lineStyle: {
                color: "#555"
              }
            },
          }],
          series: series
        };
        option.yAxis[0].min=min[0];
        if(params.id=="merchant"){
          option.yAxis[0].name=params.legend[0];
          option.yAxis.push({
            name:params.legend[1],
            splitLine: { show: false },
            axisLine: {
              lineStyle: {
                color: "#555"
              }
            },
            min:min[1]
          });
        }
        myChart.on("legendselectchanged", function(legend) {//legend click
          let index = 0,
          len = Object.keys(legend.selected).length;
          if (legend.name == "时") {
            index = 0;
          } else if (legend.name == "天") {
            if(len == 1){
              index =0 ;
            }else index = 1;
          } else if (legend.name == "月") {
            index = len - 1;
          }
          if(params.id=="cardCreate"){
            vm.off.cardTry_index=index
          }else if(params.id=="onlineUser"){
            vm.off.onlineUser_index=index
          }else if(params.id=="rechargeMoney"){
            vm.off.recharge_index=index
          }else if(params.id=="openCardSuccess"){
            vm.off.cardSuccess_index=index
          }
          option.xAxis.data = params.category[index];
          if(params.id!="merchant"){
            option.yAxis[0].min=min[index];
          }
          myChart.setOption(option);
        });
        myChart.setOption(option);
        myChart.hideLoading();
        } 
      },
      getDateTime(v){
        return getDateTime(v);
      },
    }
  };
</script>

