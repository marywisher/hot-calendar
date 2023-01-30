<template>
	<view>
		<view class="cal-title"><label>{{innerTitle}}</label></view>
		<view class="cal-bg">
			<view class="cal-week" v-for="indexWeek in weekNum" :key="indexWeek">
				<view class="cal-cell" :class="{'mark-cell': borderStyle(indexWeek, indexDay), 'unvisible-cell': (cellStyle(indexWeek, indexDay) == levelUnlimit)}" :style="{background: itemColor[cellStyle(indexWeek, indexDay)], width: cellWidth, height: cellWidth, lineHeight: cellWidth, borderRadius: cellRadius, marginBottom: cellMargin}" v-for="indexDay in 7" :key="indexDay" @tap="itemClick(cellStyle(indexWeek, indexDay), indexWeek, indexDay)">{{cellDay(indexWeek, indexDay)}}</view>
			</view>
		</view>
		<view class="cal-lab">
			<label :style="{minWidth: cellWidth}" v-for="(monthStr, index) in monthArr" :key="index">{{monthStr}}</label>
		</view>
	</view>
</template>

<script>
	export default {
		name: "hot-calendar",
		props: {
			title:{
				type: String,
				default: ''
			},
			//1em = 14px
			cellWidth: {//需要扣除2px cell border
				type: String,
				default: '1.64em'
			},
			cellRadius:{//border-radius
				type: String,
				default: '0'
			},
			cellMargin: {//需要扣除2px cell border
				type: String,
				default: '0.21em'
			},
			cellData:{
				type: Array,
				default: []
			},
			showDay:{//日历是否显示日期
				type: Boolean,
				default: false
			},
			showEarly:{//早于记录的日历是否显示
				type: Boolean,
				default: false
			},
			level1:{
				type: Number,
				default: 1
			},
			level2:{
				type: Number,
				default: 3
			},
			level3:{
				type: Number,
				default: 6
			},
			level4:{
				type: Number,
				default: 10
			},
			levelUnlimit:{
				type: Number,
				default: 100
			},
			pointDay: {
				type: String,
				default: ''
			}
		},
		data() {
			return {
				innerTitle: '',
				todayDay: '',
				weekNum: 10,
				monthArr: [],
				dayArr: [],
				crtWeek: 10,
				crtDay: 0
			};
		},
		created() {
			this.init()
		},
		watch:{
			title(value){
				if (this.title != '') this.innerTitle = this.title;
			},
			cellData(){
				this.monthArr = [];
				this.dayArr = [];
				this.init();
			}
		},
		computed: {
			itemColor: function(){
				return {100: 'aliceblue',
					0: 'rgb(233,233,233)', 
					1: '#9badb5',
					3: '#71919f',
					6: '#39687c',
					10: '#235368'}
			},
			cellStyle(){
				return function(i, j){
					// #ifdef H5
					i -= 1;
					j -= 1;
					// #endif
					
					let d = new Date();
					let cellDate = new Date(this.dayArr[i][j]);
					if(cellDate > d) {//过晚不显示
						//console.log('future');
						return this.levelUnlimit;
					}
					if(!Array.isArray(this.cellData) || this.cellData.length == 0) return 0;
					//console.log(this.cellData)
					let inputData = this.cellData[i][j];
					if(inputData >= this.levelUnlimit){
						if(this.showEarly == false) return this.levelUnlimit;//过早不显示
						else return 0;
					} 
					else if(inputData >= this.level4) return this.level4;
					else if(inputData >= this.level3) return this.level3;
					else if(inputData >= this.level2) return this.level2;
					else if(inputData >= this.level1) return this.level1;
					else if(inputData == 0) return 0;
					return this.levelUnlimit;
					
				}
			},
			borderStyle(){
				return function(i, j){
					//if(!Array.isArray(this.dayArr) || this.dayArr.length == 0) return false;
					if(i == this.crtWeek && j + 1 == this.crtDay){
						//console.log(i, j)
						return true;
					}else{
						return false;
					}
				}
			},
			cellDay(){
				return function(i, j){
					// #ifdef H5
					i -= 1;
					j -= 1;
					// #endif
					if(this.showDay) return this.dayArr[i][j].substr(8, 2)
					else return ''
				}
			}
		},
		methods:{
			itemClick(cnt, crtWeek, crtDay){
				//console.log(date, cnt);
				if(cnt < this.levelUnlimit){
					this.crtDay = crtDay + 1;
					this.crtWeek = crtWeek;
					
					// #ifdef H5
					crtDay -= 1;
					crtWeek -= 1;
					// #endif
					
					this.$emit('cellTap', this.dayArr[crtWeek][crtDay]);
				} 
			},
			init(){
				let d = new Date();
				this.innerTitle = d.getFullYear() + '年';
				
				if(this.cellData.length)
					this.weekNum = Math.max(this.cellData.length, this.weekNum);
				
				let y = d.getFullYear();
				let m = d.getMonth() + 1 < 10 ? "0" + (d.getMonth() + 1) : d.getMonth() + 1;
				let r = d.getDate() < 10 ? "0" + d.getDate() : d.getDate();
				if(this.pointDay == ''){
					this.crtWeek = this.weekNum;
					this.crtDay = d.getDay() + 2;
				}
				//console.log(this.crtDay)
				//this.todayDay = y + '-' + m + '-' + r;
				//console.log(this.todayDay)
				//console.log(this.weekNum)
				//console.log(this.cellData.length)
				d.setDate(d.getDate() - (d.getDay() + 7 * (this.weekNum - 1) + 1));//推算第一天的前一天日期
				let tmpMon = ''
				for(let i = 0; i < this.weekNum; i ++){
					this.dayArr[i] = [];
					for(let j = 0; j < 7; j ++){
						d.setDate(d.getDate() + 1)
						y = d.getFullYear();
						m = d.getMonth() + 1 < 10 ? "0" + (d.getMonth() + 1) : d.getMonth() + 1;
						r = d.getDate() < 10 ? "0" + d.getDate() : d.getDate();
						
						this.dayArr[i][j] = y + '-' + m + '-' + r;
					}
					
					if(tmpMon != m) {
						this.monthArr.push(m + '月')
						tmpMon = m
					}
					else this.monthArr.push('')
				}
			}
		}		
	}
</script>

<style>
	.cal-bg, .cal-lab{
		display: flex;
		justify-content: space-between;
		padding: 0 10rpx;
	}
	.cal-title{
		text-align: center;
	}
	.cal-lab{
		font-size: 20rpx;
		min-width: 46rpx;
	}
	.cal-cell{
		border: 1px solid aliceblue;
		cursor: pointer;
		text-align: center;
		color: aliceblue;
		font-size: 24rpx;
	}
	.unvisible-cell, label{
		cursor: unset;
	}
	.mark-cell{
		border-color: #001f2c;
	}
</style>
