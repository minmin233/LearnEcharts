# LearnEcharts
数据可视化展板

## 学习目的
1. 可视化面板布局适配屏幕
2. 利用Echarts实现柱状图，折线图，饼状图，地图展示

数据可视化的主要目的：借助于图形化手段，清晰有效地表达与沟通信息，揭示蕴含在数据中的规律和道理

## 技术栈
1. 基于 flexible.js+rem 智能大屏适配
2. VScode cssrem 插件（生成rem单位）
3. Flex 布局（三列）
4. Less 使用
5. 基于 Echarts 数据可视化展示
6. Echarts 柱状图，折线图，饼形图数据设置
7. Echarts 地图引入

## 布局
### 01- 案例适配方案
* 设计稿是1920px
* easy less 插件
* flexible.js 把屏幕分为 24 等份，默认是10等份
* cssrem 插件的基准值是 80px，插件-配置按钮---配置扩展设置--Root Font Size 里面 设置，但是别忘记重启vscode软件保证生效

### 02-基础设置
* css初始化
  * border-box 宽度包括border padding content
  * 给li去小圆点
  * 声明字体
* body 设置背景图 ，缩放为 100% ， 行高1.15

### 03-header 布局
* 高度为100px
* 背景图，在容器内显示
* 缩放比例为 100%
* h1 标题部分 白色 38像素 居中显示 行高为 80像素
* 时间模块 showTime 子绝父相 定位右侧 right 为 30px 行高为 75px 文字颜色为：rgba(255, 255, 255, 0.7) 而文字大小为 20像素

```javascript
// 格式： 当前时间：2020年3月17-0时54分14秒
<script>
            var t = null;
            t = setTimeout(time, 1000);//開始运行
            function time() {
                clearTimeout(t);//清除定时器
                dt = new Date();
                var y = dt.getFullYear();
                var mt = dt.getMonth() + 1;
                var day = dt.getDate();
                var h = dt.getHours();//获取时
                var m = dt.getMinutes();//获取分
                var s = dt.getSeconds();//获取秒
                document.querySelector(".showTime").innerHTML = '当前时间：' + y + "年" + mt + "月" + day + "-" + h + "时" + m + "分" + s + "秒";
                t = setTimeout(time, 1000); //设定定时器，循环运行     
            }
 </script>
```

### 04-mainbox 主体模块
* 需要一个上左右的10px 的内边距
* column 列容器，分三列，占比 3：5：3

### 05-公共面板模块 panel
* 高度为 310px
* 1像素的 1px solid rgba(25, 186, 139, 0.17) 边框
* 有line.jpg 背景图片
* padding为 上为 0 左右 15px 下为 40px
* 下外边距是 15px
* 利用panel 盒子 before 和after 制作上面两个角 大小为 10px 线条为 2px solid #02a6b5
* 新加一个盒子before 和after 制作下侧两个角 宽度高度为 10px

### 06-柱形图 bar 模块(布局)
* 标题模块 h2 高度为 48px 文字颜色为白色 文字大小为 20px
* 图标内容模块 chart 高度 240px
* 以上可以作为panel公共样式部分

### 07-中间布局
* 上面是no 数字模块
1. 数字模块 no 有个背景颜色 rgba(101, 132, 226, 0.1); 有个15像素的内边距
2. 注意中间列 column 有个 左右 10px 下 15px 的外边距
3. no 模块里面上下划分 上面是数字（no-hd) 下面 是 相关文字说明(no-bd)
4. no-hd 数字模块 有一个边框 1px solid rgba(25, 186, 139, 0.17)
5. no-hd 数字模块 里面分为两个小li 每个小li高度为 80px 文字大小为 70px 颜色为 #ffeb7b 字体是图标字体 electronicFont
6. no-hd 利用 after 和 before制作2个小角， 边框 2px solid #02a6b5 宽度为 30px 高度为 10px
7. 小竖线 给 第一个小li after 就可以 1px宽 背景颜色为 rgba(255, 255, 255, 0.2); 高度 50% top 25% 即可
8. no-bd 里面也有两个小li 高度为 40px 文字颜色为 rgba(255, 255, 255, 0.7) 文字大小为 18px 上内边距为 10px

* 下面是map 地图模块

1. 地图模块高度为 810px 里面包含4个盒子 chart放地图模块 球体图片 旋转1 旋转2
2. 球体图片模块 map1 大小为 518px 要加背景图片 因为要缩放100% 定位到最中央 透明度 .3
3. 旋转1 map 2 大小为 643px 要加背景图片 因为要缩放100% 定位到中央 透明度 .6 做旋转动画 旋转位置不变 translate 利用z-index压住球体
4. 旋转2 map3 大小为 566px 要加背景图片 因为要缩放100% 定位到中央 旋转动画 注意是逆时针


## ECharts引入
### 01-Echarts-介绍
常见的数据可视化库：

* D3.js 目前 Web 端评价最高的 Javascript 可视化工具库(入手难)
* ECharts.js 百度出品的一个开源 Javascript 数据可视化库
* Highcharts.js 国外的前端数据可视化库，非商用免费，被许多国外大公司所使用
* AntV 蚂蚁金服全新一代数据可视化解决方案 等等
* Highcharts 和 Echarts 就像是 Office 和 WPS 的关系

大白话：
* 是一个JS插件
* 底层依赖矢量图形库 [ZRender](https://github.com/ecomfe/zrender)
* 性能好可流畅运行PC与移动设备
* 兼容主流浏览器
* 提供很多常用图表，且可定制。
  * 折线图、柱状图、散点图、饼图、K线图
  
### 02-Echarts-体验 [官网地址](https://www.echartsjs.com/zh/index.html)
  * 下载并引入echarts.js文件 因为图表都依赖这个插件
  * 准备一个有大小的DOM容器 生成的图表会放在这个容器内
  * 初始化echarts实例对象 `echarts.init(dom容器)`
  * 指定配置项和数据(option)
  * 将配置项设置给echarts实例对象
![配置](https://img-blog.csdnimg.cn/2020052816161643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjE3NDgw,size_16,color_FFFFFF,t_70)
* 修改大小找grid containLabel(grid 区域是否包含坐标轴的刻度标签,默认不包含)为true的情况下,无法使图表紧贴着画布显示,但可以防止标签长度动态变化时溢出容器或者覆盖其他组件,将containLabel设置为false即可解决;

立即执行函数
为了防止变量污染，减少命名冲突，我们可以采取立即执行函数的写法。
里面的变量都是局部变量。

### 03-柱状图图表1（两大步骤）
* 官网找到类似实例， 适当分析，并且引入到HTML页面中
* 根据需求定制图表
1. 修改图表柱形颜色color #2f89cf
2. 修改图表大小grid top 为 10px bottom 为 4% grid决定我们的柱状图的大小
3. X轴相关设置 xAxis
   * 文本颜色axisLabel设置为 rgba(255,255,255,.6) 字体大小为 12px
   * X轴线的样式axisLine 不显示
4. Y 轴相关定制 yAxis
   - 文本颜色设置为   rgba(255,255,255,.6)   字体大小为 12px
   - Y 轴线条样式lineStyle 更改为  1像素的  rgba(255,255,255,.1) 边框
   - 分隔线splitLine的颜色修饰为  1像素的  rgba(255,255,255,.1)  
5. 修改柱形itemStyle为圆角以及柱子宽度 series 里面设置
6. 更换对应数据

```javascript
// x轴中更换data数据
 data: [ "旅游行业","教育培训", "游戏行业", "医疗行业", "电商行业", "社交行业", "金融行业" ],
// series 更换数据 每一项的真实数据 一般从后台请求 ajax
 data: [200, 300, 300, 900, 1500, 1200, 600]
```
7. 让图表跟随屏幕自适应

```javascript
window.addEventListener("resize", function() {
    myChart.resize();
  });
```
### 04-柱状图图表2
* 修改图形大小 grid
* 不显示x轴的相关信息
* y轴相关信息
  1. 不显示y轴线和相关刻度
  2. y轴文字的颜色设置为白色
* 修改第一组柱子相关样式（条状）
  1. 设置第一组柱子内百分比显示数据
  2. 设置第一组柱子不同颜色
*  修改第二组柱子的相关配置（框状）采取定位的方式
* 给y轴添加第二组数据
* 设置两组柱子层叠以及更换数据

```javascript
series: [
			{
				name: "条",
				type: "bar",
				data: [70, 34, 60, 78, 69],
				yAxisIndex: 0,
				// 柱子的样式
				itemStyle: {
					// 修改第一组柱子的圆角
					barBorderRadius: 20,
					// 此时的color 可以修改柱子的颜色
					color: function (params) {
						// params 传进来的是柱子对象
						// console.log(params);
						// dataIndex 是当前柱子的索引号
						return myColor[params.dataIndex];
					}
				},
				// 柱子之间的距离
				barCategoryGap: 50,
				//柱子的宽度
				barWidth: 10,
				// 显示柱子内的文字
				label: {
					show: true,
					position: "inside",
					// {c} 会自动的解析为 数据  data里面的数据
					formatter: "{c}%"
				}
			},
			{
				name: "框",
				type: "bar",
				barCategoryGap: 50,
				barWidth: 15,
				yAxisIndex: 1,
				data: [100, 100, 100, 100, 100],
				itemStyle: {
					color: "none",
					borderColor: "#00c1de",
					borderWidth: 3,
					barBorderRadius: 15
				}
			}
		]
```

### 05-折线图图表1
*  修改折线图大小，显示边框设置颜色：#012f4a，并且显示刻度标签。
* 修改图例组件中的文字颜色 #4c9bfd， 距离右侧 right 为 10%
* x轴相关配置
	1. 刻度去除
	2. x轴刻度标签字体颜色：#4c9bfd
	3. 剔除x坐标轴线颜色（将来使用Y轴分割线)
	4. 轴两端是不需要内间距 boundaryGap
* y轴的定制
	1. 刻度去除
	2. 字体颜色：#4c9bfd
	3. 分割线颜色：#012f4a
* 两条线形图定制
	1. 颜色分别：#00f2f1 #ed3f35
	2. 把折线修饰为圆滑 series 数据中添加 smooth 为 true
* 配置数据
* 新增需求 点击 2019年 2020年 数据发生变化
* tab栏切换事件 
	1. 引入jQuery库
	2. 点击2019按钮 需要把 series 第一个对象里面的data 换 2019年对象里面data[0] 需要把 series 第二个对象里面的data 换成 2019年对象里面data[1]
* 2020 按钮同样道理

```javascript
<h2>
	折线图-人员变化
	<a href="javascript:;">2019</a>
	<a href="javascript:;">2020</a>
</h2>
```

```javascript
var yearData = [
		{
			year: '2019',  // 年份
			data: [  // 两个数组是因为有两条线
				[24, 40, 101, 134, 90, 230, 210, 230, 120, 230, 210, 120],
				[40, 64, 191, 324, 290, 330, 310, 213, 180, 200, 180, 79]
			]
		},
		{
			year: '2020',  // 年份
			data: [  // 两个数组是因为有两条线
				[123, 175, 112, 197, 121, 67, 98, 21, 43, 64, 76, 38],
				[143, 131, 165, 123, 178, 21, 82, 64, 43, 60, 19, 34]
			]
		}
	];
```

```javascript
series: [
			{
				name: '新增粉丝',
				data: yearData[0].data[0],
				type: 'line',
				// 折线修饰为圆滑
				smooth: true,
			},
			{
				name: '新增游客',
				data: yearData[0].data[1],
				type: 'line',
				// 折线修饰为圆滑
				smooth: true,
			},
		]
```

```javascript
// 5.点击切换效果
	$('.line h2').on('click','a',function(){
		// console.log($(this).index());
		// 点击a之后，根据当前a的索引号找到对应的yearData对象
		// console.log(yearData[$(this).index()]);
		var obj = yearData[$(this).index()]
		option.series[0].data = obj.data[0]
		option.series[1].data = obj.data[1]
		// 需要重新渲染
		myChart.setOption(option);	
	})
```

### 06-折线图图表2
* 更换图例组件文字颜色 rgba(255,255,255,.5) 文字大小为12
* 修改图表大小
* 修改x轴相关配置
	1. 修改文本颜色为rgba(255,255,255,.6) 文字大小为 12
	2. x轴线的颜色为 rgba(255,255,255,.2)
* 修改y轴的相关配置
* 修改两个线模块配置(注意在series 里面定制)
* 更换数据

```javascript
series: [
			{
				name: "播放量",
				type: 'line',
				data: [30, 40, 30, 40, 30, 40, 30, 60, 20, 40, 30, 40, 30, 40, 30, 40, 30, 60, 20, 40, 30, 40, 30, 40, 30, 40, 20, 60, 50, 40],
				//第一条 线是圆滑
				smooth: true,
				// 单独修改线的样式
				lineStyle: {
					color: "#0184d5",
					width: 2
				},
				areaStyle: {
					// 渐变色，只需要复制即可
					color: new echarts.graphic.LinearGradient(
						0,
						0,
						0,
						1,
						[
							{
								offset: 0,
								color: "rgba(1, 132, 213, 0.4)"   // 渐变色的起始颜色
							},
							{
								offset: 0.8,
								color: "rgba(1, 132, 213, 0.1)"   // 渐变线的结束颜色
							}
						],
						false
					),
					shadowColor: "rgba(0, 0, 0, 0.1)"
				},
				// 设置拐点 小圆点
				symbol: "circle",
				// 拐点大小
				symbolSize: 8,
				// 设置拐点颜色以及边框
				itemStyle: {
					color: "#0184d5",
					borderColor: "rgba(221, 220, 107, .1)",
					borderWidth: 12
				},
				// 开始不显示拐点， 鼠标经过显示
				showSymbol: false,
			},
			]
```


### 07-饼形图图表1
* 修改图例组件
	1. 在底部并且居中显示。
	2. 每个小图标的宽度和高度修改为 10px
	3. 文字大小为12 颜色 rgba(255,255,255,.5)
* 修改水平居中 垂直居中
* 修改内圆半径和外圆半径为 ["40%", "60%"]  友情提示，带有直角坐标系的比如折线图柱状图是 grid修改图形大小，而我们饼形图是通过 radius 修改大小
* 更换数据
* 更换颜色

### 08-饼形图2
* 颜色设置
* 修改饼形图大小 ( series对象)
* 把饼形图的显示模式改为 半径模式
* 数据使用更换（series对象 里面 data对象）
* 字体略小些 10 px ( series对象里面设置 ) label 对象设置
* 防止缩放的时候，引导线过长。引导线略短些 (series对象里面的 labelLine 对象设置 )  连接图表 6 px 连接文字 8 px

### 09-地图
实现步骤：
* 第一需要下载china.js提供中国地图的js文件
* 第二个因为里面代码比较多，我们新建一个新的js文件 myMap.js 引入
* 使用社区提供的配置即可。

需要修改：
* 去掉标题组件
* 去掉背景颜色
* 修改地图省份背景 #142957 areaColor 里面做修改
地图放大通过 zoom 设置为1.2即可

## 约束缩放
/* 约束屏幕尺寸 */

```javascript
@media screen and(max-width: 1024px) {
	html {
		font-size: 42px!important;
	}
}
@media screen and(min-width: 1920px) {
	html {
		font-size: 80px!important;
	}
}
```
