<style scoped>
  .m-footD-btn>a{margin-right: 0.2rem;}
</style>
<template>
<section class="g-list-box" id="details">
	<header class="g-lis-head">
		<a class="m-details-back u-icon-back" @click="close"></a>
  	  	<div class="m-footD-btn" v-if="list.status==1">
			<a class="f-btn f-btn-success" @click="audit(2)">通过</a>
			<a class="f-btn f-btn-danger" @click="refuse()">拒绝</a>
	  	</div>
  	</header>
	<div class="g-box">
		<table class="g-list-table">
			<tbody>
				<tr>
					<td>
						<table class="g-inner-table">
							<tbody>
								<tr><td>订单号：</td><td>{{ list.orderId }}</td></tr>
								<tr><td>申请业务：</td>
									<td v-if="list.type==3">联通售卡</td>
									<td v-if="list.type==4">远特售卡</td>
								</tr>
								<tr><td>审核状态：</td>
									<td v-if="list.status==1" class="f-c-yellow">待审核</td>
									<td v-if="list.status==2" class="f-c-green">通过</td>
									<td v-if="list.status==3" class="f-c-red">拒绝</td>
								</tr>
								<tr><td>申请时间：</td><td>{{ $parent.getDateTime(list.createTime)[6] }}</td></tr>
								<tr v-if="type==2"><td>审核时间：</td><td>{{ $parent.getDateTime(list.modifyTime)[6] }}</td></tr>
								<tr><td>商户名称：</td><td>{{ list.companyName }}<a href="javascript:void(0)" @click="detailsMerchant" class="details m-l">查看详情</a></td></tr>
								<tr><td>商户ID：</td><td>{{ list.dealerId }}</td></tr>
								<tr><td>原业务范围：</td>
									<td v-if="list.oldType==1">A类远特售卡</td>
									<td v-if="list.oldType==2">B类联通售卡</td>
								</tr>
								<tr><td>申请人姓名：</td><td>{{ list.userName }}</td></tr>
								<tr><td>申请人ID：</td><td>{{ list.userId }}<a href="javascript:void(0)" @click="detailsUser" class="details m-l">查看详情</a></td></tr>
								<tr><td>申请人手机号：</td><td>{{ list.userPhone }}</td></tr>
								<tr v-if="type==2"><td>审核人：</td><td>{{ list.customerName }}</td></tr>
								<tr v-if="type==2"><td>审核人ID：</td><td>{{ list.customerId }}</td></tr>
								<tr v-if="list.status==3"><td>拒绝原因：</td><td>{{ list.reason }}</td></tr>
							</tbody>
						</table>
					</td>
					<td class="m-box-img m-meida-640up">
						<ImgZoom :imgData="imgData"></ImgZoom>
					</td>
				</tr>
				<tr class="m-box-img m-meida-640down">
					<img v-for="item in imgData" :src="item.src">
				</tr>
			</tbody>
  		</table>
  	</div>
  <um-details-view v-if="isShowDetails" :type="typeDetails" :list="detailsList" :dealerId="list.dealerId">
	
  </um-details-view>
</section>
</template>
<script>
import "../assets/css/cardOrderDetails.css";
import ImgZoom from '../components/ImgZoom';
import detailsView from '../components/cardOrderDetailsAlert';
export default{
	name:'orderDetails',
	props:{
		list:Object,
		type:Number,
		number:String,
	},
	data(){
		return{
			imgData:[],//当前订单的图片
			isShowDetails:0,
			typeDetails:0,
			detailsList:''
		}
	},
	components:{
		'um-details-view':detailsView,
		'ImgZoom':ImgZoom
	},
	created:function(){
		var vm=this;
		vm.imgData=[{'src':vm.list.img,'name':'手签名'}];
	},
	methods:{
		close:function(){
			this.$parent.off.details=false
		},
		audit:function(result,reason,cb){//复审同意
			var vm=this;
			vm.AJAX('w/attribute/audit',{"orderId":vm.list.orderId,"result":result,"reason":reason||''},function(data){
				layer.open({
		            content:'操作成功',
		            skin: 'msg',
		            time: 4,
		            msgSkin:'success',
		        });
				vm.list.status=result;
				vm.$parent.list.splice((parseInt(vm.number)+1),1);
				cb&&cb();
			});
		},
		refuse:function(){
			var vm=this,ww=window.innerWidth,wwSet,popIndex;
			ww<=640 ? wwSet='width:98%' : wwSet='max-width:645px';
			popIndex=layer.open({
				content:'<div class="layer-refuse-pop"><textarea maxlength="30" id="reason"></textarea></div>',
				btn:['确定','取消'],
				type:1,
				style:wwSet,
				title:['请输入拒绝理由'],
				yes:function(){
					var reason=document.getElementById('reason').value;
					if(!reason)return false;
					vm.audit(3,reason,function(){
						layer.close(popIndex);
					})
				}
			});
		},
		detailsUser:function(){//申请人详情
			var vm=this;
			vm.AJAX('w/audit/getUserInfo',{"userId":vm.list.userId},function(data){
				vm.detailsList=data.data;
				vm.isShowDetails=true;
				vm.typeDetails=1;
			});
		},
		detailsMerchant:function(){//商户详情
			var vm=this;
			vm.AJAX('w/audit/getMerchantInfo',{"dealerId":vm.list.dealerId},function(data){
				vm.detailsList=data.data;
				vm.isShowDetails=true;
				vm.typeDetails=2;
			})
		},
		
	}
}
</script>

