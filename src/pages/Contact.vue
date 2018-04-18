<template>
	<div class="contact-main">
		<v-search></v-search>
			<div class="contact-content" v-for="(user,key) in contactList">
				<!-- eng为首字母 -->
				<div class="contact-box" v-if="key==0 || (key>0 &&contactList[key].eng != contactList[key-1].eng)">
					<div class="letterTitle">{{contactList[key].eng}}</div>
				</div>	
				
				<router-link to="/business">
					<div class="contact-list">
					<div class="list">
						<div class="list-left">
							<div class="list-header"></div>
							<div class="list-name">{{user.name}}</div>
						</div>
						<div class="list-phone">{{user.mobile}}</div>
					</div>				
				</div>
				</router-link>
			</div>
		
		<v-foot :pass=show></v-foot>

	</div>
</template>

<script>
import search from "../components/search/search"
import customerFoot from '@/components/customer-foot/customerFoot'
import prompt from '@/components/suggestdelete/delete'
export default{
	name :'contact',
	data (){
		return {
			show:1,
			contactList :[
				
        	]
		}
	},
	components:{
		'v-search':search,
		'v-foot': customerFoot,
	},
	created:function(){
		this.getCon()       //获取联系人列表
	},
	methods: {
		engsort: function(arr){
    		var colId="eng";
    
    		var asc = function(x,y)  
    		{  
       		 	return (x[colId] > y[colId]) ? 1 : -1  
    		}  
    
    		return arr.sort(asc);
		},
        getCon: function() {
        	var _this=this;
            _this.$http.get('/list/api/linkmaninfo/list?page=1&limit=20',{headers:{'token':'b8ddab5b7aaddf4286f129db2576c77c'}})
                .then((response) => {
                    var data = response.data;
                    _this.contactList = data.page.list;//获取数据

                    this.engsort(_this.contactList);
                })
                .catch(function(response) {
                    console.log(response)
                })
        }

	}

}
</script>

<style lang="stylus" scoped>
@import '../common/stylus/variable.styl'
	.contact-main 
		width:100%
		height:$height
		background: $contacat-background
	
	.contact-content
		width:100%
		height: auto
	.letterTitle
		width: auto
		height:100%
		margin-left:10px
		font-family: PingFangSC-Regular
		font-size: 14px
		color: #555555
		letter-spacing: -0.34px
	.contact-box
		width:100%
		height:22px
		line-height:22px
	.contact-list
		width:100%
		height:auto
		display:flex
		flex-direction:column
		margin-bottom:1px
	.list
		width:100%
		height:54px
		background:#fff
	.list:last-child
		margin-bottom:0
		display:flex
		justify-content: space-between
		align-items:center
	.list-left
		width:108px
		height:100%
		margin-left:10px
		display: flex
		align-items:center
	.list-header
		width:36px
		height:36px
		background: #D8D8D8
		border: 1px solid #979797
		border-radius: 4px
	.list-name
		margin-left:10px
		font-family: PingFangSC-Regular
		font-size: 14px
		color: #010101
		letter-spacing:0
	.list-phone
		margin-right:20px
		font-family: PingFangSC-Regular
		font-size: 14px
		color: #010101
		letter-spacing:0
</style>