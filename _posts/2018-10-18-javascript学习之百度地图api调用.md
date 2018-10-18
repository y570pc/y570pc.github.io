---
layout: post
title:  "javascript学习之百度地图api调用"
category: 'bigdata'
tags: javascript 百度地图api
author: y570pc
image: "/img/2018-10-18-01.jpg"
introduction: 本人一直有各种收藏癖，收藏的想要去的地方多了以后，就想着怎么分类管理。现有的各种地图APP不能满足我的要求，于是决定自己动手解决。
---


## 功能

#### 已实现功能

\item[$\bigstar$] 基于浏览器定位
\item[$\bigstar$] 多点标注
\item[$\bigstar$] 城市切换
\item[$\bigstar$] 图文信息窗口

#### 未实现功能

\item[$\bigstar$] 分类筛选
\item[$\bigstar$] 信息窗口滑块
\item[$\bigstar$] 分类标注
\item[$\bigstar$] 基于控件添加地点信息
\item[$\bigstar$] 数据分离

#### 代码
{% highlight js%}
<!DOCTYPE html>
<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
	<style type="text/css">
	body, html,#allmap {width: 100%;height: 100%;overflow: hidden;margin:0;font-family:"FangSong";}
	</style>
	<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=GAl1mcNmnu1KHGWLAq4CD8IG7Gjz18n9"></script>
	<title>分类地图</title>
</head>
<body>
	<div id="allmap"></div>
</body>
</html>
<script type="text/javascript">
	// 百度地图API功能
	var map = new BMap.Map("allmap");
	var center = new BMap.Point(116.404, 39.915);
	map.centerAndZoom(center, 15);
    map.enableScrollWheelZoom();
	var data_info = [[108.9396,34.2762,
	"<h4 style='margin:0 0 0px 0;padding:0.1em 0'>洒金桥</h4>" + 
	"<img style='float:right;margin:2px' id='imgDemo' src='http://img.mp.itc.cn/upload/20160518/922c9d68d1d443d58ab38f0952b6ebeb_th.jpg' width='100' height='75' />" + 
	"<p style='margin:0;line-height:1.5;font-size:10px;text-indent:0em'><b>标签：</b>美食街</p>"+
	"<p style='margin:0;line-height:1.5;font-size:10px;text-indent:0em'><b>评分：</b>无</p>"+
	"<p style='margin:0;line-height:1.5;font-size:10px;text-indent:0em'><b>简介：</b>天安门坐落在中国北京市中心,故宫的南侧,与天安门广场隔长安街相望,是清朝皇城的大门...</p>"],
					 [116.406605,39.921585,"地址：北京市东城区东华门大街"],
					 [116.412222,39.912345,"地址：北京市东城区正义路甲5号"]
					];
	var opts = {
				width : 200,     // 信息窗口宽度
				height: 150,     // 信息窗口高度
				enableMessage:true//设置允许信息窗发送短息
			   };
			   

    //创建检索信息窗口对象
	for(var i=0;i<data_info.length;i++){
	    var pt = new BMap.Point(data_info[i][0],data_info[i][1]);
		var marker = new BMap.Marker(pt);  // 创建标注
		var content = data_info[i][2];
		map.addOverlay(marker);               // 将标注添加到地图中
		addClickHandler(content,marker);
	}
	function addClickHandler(content,marker){
		marker.addEventListener("click",function(e){
			openInfo(content,e)}
		);
	}
	function openInfo(content,e){
		var p = e.target;
		var point = new BMap.Point(p.getPosition().lng, p.getPosition().lat);
		var infoWindow = new BMap.InfoWindow(content,opts);  // 创建信息窗口对象 
		map.openInfoWindow(infoWindow,point); //开启信息窗口
	}
	//添加城市控件
	var size = new BMap.Size(10, 20);
	map.addControl(new BMap.CityListControl({
    anchor: BMAP_ANCHOR_TOP_LEFT,
    offset: size,
    // 切换城市之间事件
    // onChangeBefore: function(){
    //    alert('before');
    // },
    // 切换城市之后事件
    // onChangeAfter:function(){
    //   alert('after');
    // }
    }));
	//根据浏览器定位
	var geolocation = new BMap.Geolocation();
	geolocation.getCurrentPosition(function(r){
		if(this.getStatus() == BMAP_STATUS_SUCCESS){
			var mk = new BMap.Marker(r.point);
			map.addOverlay(mk);
			map.panTo(r.point);
			alert('您的位置：'+r.point.lng+','+r.point.lat);
		}
		else {
			alert('failed'+this.getStatus());
		}        
	},{enableHighAccuracy: true});
</script>
{% endhighlight %}

## 参考资料
[1]. [百度地图api示例](http://lbsyun.baidu.com/jsdemo.htm)
