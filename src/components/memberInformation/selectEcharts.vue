<template>
	<!--
		此模块是绘制一个饼图
		饼图主要是通过扇形的弧度表现不同类目的数据在总和中的占比，它的数据格式比柱状图更简单,只有一维的数值。
		因为不在直角坐标系上，所以也不需要xAxis和yAxis
	-->
	<div  :style="{width: '1100px'}">
		<div id="myChart" :style="{width: '1100px', height: '500px'}"></div>
	</div>
	
</template>

<script>
	export default {
		data() {
			return {
			url: "http://47.100.245.30:8080",
			}
		},
		mounted() {
			this.drawLine();
		},
		methods: {
			drawLine() {
					const th = this;
				axios.get(th.url + '/memberInformation/homeReport')
				.then(function(res) {
					if(res.data.code === 1224 && res.data.data != null){
						th.xr(res);
					}else{
						th.xrs();
					}
					})
			},
			xr(res){
				const th = this;
				var person = new Array();
				var type = new Array();
				console.log(res.data.data[0]);
				console.log('=======')
				
				for(let i =0 ;i < res.data.data.length;i++){
					person.push({name: res.data.data[i].dName, value: res.data.data[i].count});
					type.push(res.data.data[i].dName);
				}
				
// 				res.data.data.forEcath(function(item){
// 					person.push({name: item.dName, value: item.count});
// 					type.push(item.dName);
// 				});
				var option = {
					title: {
						text: ' ',
						subtext: '软件学院(虚拟数据)',
						x: 'center'
					},
					tooltip: {
						trigger: 'item',
						formatter: "{a} <br/>{b} : {c} ({d}%)"
					},
					legend: {
						type: 'scroll',
						orient: 'vertical',
						right: 10,
						top: 10,
						bottom: 10,
						data: type,
						selected: false
					},
					series: [{
						name: '部门',
						type: 'pie',
						//设置图形大小
						radius: '65%',
						center: ['50%', '50%'],
						data: person,
						itemStyle: {
							emphasis: {
								shadowBlur: 10,
								shadowOffsetX: 0,
								shadowColor: 'rgba(0, 0, 0, 0.5)'
							}
						}
					}]
				};
				// 基于准备好的dom，初始化echarts实例
				let myChart = th.$echarts.init(document.getElementById('myChart'))
				// 绘制图表
				myChart.setOption(option);
				
			},
			xrs(){
				const th = this;
				var person = [
					{name: "指导老师", value: 4},
					{name: "主席团", value: 4},
					{name: "纪检部", value: 20},
					{name: "生活部", value: 25},
					{name: "播音部", value: 10},
					{name: "文体部", value: 28},
					{name: "秘书部", value: 6}];
				var type = ["指导老师","主席团", "纪检部","生活部","播音部","文体部", "秘书部"];
			
				
				var option = {
					title: {
						text: '南方软件学院',
						subtext: '软件学院(虚拟数据)',
						x: 'center',
					},
					/**
					 * tooltip 提示框的内容
					 *
					 */
					tooltip: {
						/**
						 * trigger:'item' 默认的提示信息
						 */
						trigger: 'item',
						/**
						 * 自定义效果
						 */
						formatter: "{a} <br/>{b} : {c} ({d}%)"
					},
					/**
					 * legend也有多种属性
					 * type 有plain（普通简单样式）和scroll(可滚动翻页，当图例数量较多时使用)两种
					 * 应对legend的data进行赋值，一般data为数组 当series的name和legend一一对应时，即可显示相应的legend
					 */
					legend: {
						type: 'scroll',
						orient: 'vertical',
						right: 10,
						top: 10,
						bottom: 10,
						data: type,
						selected: true
					},
					series: [{
						name: '部门',
						type: 'pie',
						/**
						 * 将饼图更改为南丁格尔图。
						 * 南丁格尔图会通过半径表示数据的大小。
						 */
						roseType:'angle',
						//设置图形大小
						radius: '65%',
						center: ['50%', '50%'],
						/**
						 * data属性值是一个包含name和value属性的对象
						 */
						data: person,
						/**
						 * ECharts中有一些通用的样式，诸如阴影，透明度，颜色，边框颜色，边框宽度等
						 * 这些样式一般都会在系列的itemStyle里设置。
						 */
						itemStyle: {
							/**
							 *emphasis是鼠标hover时候的高亮样式。
							 */
							emphasis: {
								/**
								 * 阴影的大小
								 */
								shadowBlur: 10,
								/**
								 * 阴影水平方向上的偏移
								 */
								shadowOffsetX: 0,
								/**
								 * 阴影的颜色
								 */
								shadowColor: 'rgba(0, 0, 0, 0.5)'
							}
						}
					}]
				};
				// 基于准备好的dom，初始化echarts实例
				let myChart = th.$echarts.init(document.getElementById('myChart'))
				// 绘制图表
				myChart.setOption(option);
				
			}
		}
	}
</script>
