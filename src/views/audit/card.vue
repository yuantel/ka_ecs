<!--**
  *@info 审核子模块-开卡顶层
  *@author: thinkmix
  *@date 2017-11-6
* *-->
<template>
	<section class="g-audit-menu">
		<div v-if="off.source==4&&off.auditType==0" class="g-audit-menu-c">
			<div class="m-menu-div">
				<router-link :to="{name:'realtime',params:{type:6}}" class="u-bg navBg1"><div>开卡审核</div><p><span>点击审核</span></p></router-link>
				<div class="navInfoDiv"><p>待审核<span>{{auditCount.opencard}}</span></p><p>已分配<span>{{auditCount.opencard_}}</span></p></div>
			</div>
			<div class="m-menu-div">
				<router-link :to="{name:'realtime',params:{type:8}}" class="u-bg navBg9"><div>SDK开卡审核</div><p><span>点击审核</span></p></router-link>
				<div class="navInfoDiv"><p>待审核<span>{{auditCount.sdkRealTime}}</span></p><p>已分配<span>{{auditCount.sdkRealTime_}}</span></p></div>
			</div>
			<div class="m-menu-div">
				<router-link :to="{name:'realtime',params:{type:7}}" class="u-bg navBg5"><div>过户办理</div><p><span>点击审核</span></p></router-link>
				<div class="navInfoDiv"><p>待审核<span>{{auditCount.transfer}}</span></p><p>已分配<span>{{auditCount.transfer_}}</span></p></div>
			</div>
		</div>
		<div v-if="off.source==4&&off.auditType==1" class="g-audit-menu-c">
			<div class="m-menu-div">
				<router-link :to="{name:'afterwards',params:{type:6}}" class="u-bg navBg1"><div>开卡审核</div><p><span>点击审核</span></p></router-link>
				<div class="navInfoDiv"><p>待审核<span>{{auditCount.opencardAfterwards}}</span></p><p>已分配<span>{{auditCount.opencard_Afterwards}}</span></p></div>
			</div>
		</div>
		<router-view></router-view>
	</section>
</template>
<script>
import { mapState } from 'vuex';
export default{
	name:'audit',
	data (){
		return {
			off:{
				source:4,
				auditType:1//0,实时;1,事后;
			},
		}
	},
	created:function(){
		var vm=this;
		vm.setType();
	},
	watch:{
		'$route':'setType'
	},
	computed:{
		...mapState([
			"auditCount"
		])
	},
	methods:{
		setType:function(){
			const route=this.$route;
			if(route.params.source=="realtime"){
				this.off.auditType=0;
				this.off.source=4
			}else if(route.params.source=="afterwards"){
				this.off.auditType=1;
				this.off.source=4
			}

			if(route.path.indexOf('realtime/')>-1||route.path.indexOf('afterwards/')>-1){
				this.off.source=0
			}

		}
	}
}
</script>

