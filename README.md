# weather
实时全国天气 ，自动定位城市，JavaScript

#![Alt text](https://github.com/AntonySufer/weather/blob/master/weather.png)

```js
 
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
	<meta content="width=device-width, initial-scale=1, minimum-scale=1.0, maximum-scale=1.0,user-scalable=no" name="viewport" />
    <title></title>
    <link rel="stylesheet" href="">
    <script type="text/javascript" src="./js/jquery-1.10.1.min.js"></script>
    <script type="text/javascript" src="./js/util.js"></script>
    <style>
    	body{
    		width:100%;height:1.75rem;background-color:#2b9ced;
    	}
    	*{
			padding: 0;
			margin: 0;
		}
    	.main{width:100%;height:1.75rem;background-color:#2b9ced;}
    	
    	img{
    		width: 1.3rem;
    		height: 1.3rem;
    		margin: 0.25rem 0rem 0 0.5rem;
    	}
    	.show{
    		width:3.5rem;
    		height: 1.3rem;
    		margin-left:1.5rem;
    		color:#fff;
    		margin-top: -1.7rem;
    		font-size: 0.5rem;
    	}
    	#city,#warm{
    		margin-left:0.4rem;
    	}
    	@media (max-width:320px){
			.show{
				font-size: 0.4rem!important;
				margin-top: -1.85rem!important;
			}
		}
		
    </style>
</head>
<body>
<div class="main">
   <p><img id="imgs" src=""/>  </p>
     <div class="show">
       <span id="city"></span>
       <span id="warm"></span>
    </div>
</div>
<div id="allmap"></div>
 <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=DD279b2a90afdf0ae7a3796787a0742e"></script>
 <script>
  var dataList = [
        {
             "img": "./wea_img/qing.png",
            "weather": "晴"
        },
        {
             "img": "./wea_img/duoyun.png",
            "weather": "多云"
        },
        {
            "img": "./wea_img/yin.png",
            "weather": "阴"
        },
        {
            "img": "./wea_img/zhenyu.png",
            "weather": "阵雨"
        },
        {
            "img": "./wea_img/leizhenyu.png",
            "weather": "雷阵雨"
        },
        {
            "img": "./wea_img/leizhenyubingbao.png",
            "weather": "雷阵雨伴有冰雹"
        },
        {
             "img": "./wea_img/xiaoxue.png",
            "weather": "雨夹雪"
        },
        {
            "img": "./wea_img/xiaoyu.png",
            "weather": "小雨"
        },
        {
           "img": "./wea_img/zhongyu.png",
            "weather": "中雨"
        },
        {
            "img": "./wea_img/dayu.png",
            "weather": "大雨"
        },
        {
            "img": "./wea_img/baoyu.png",
            "weather": "暴雨"
        },
        {
          "img": "./wea_img/dabaoyu.png",
            "weather": "大暴雨"
        },
        {
             "img": "./wea_img/tedabaoyu.png",
            "weather": "特大暴雨"
        },
        {
             "img": "./wea_img/zhenxue.png",
            "weather": "阵雪"
        },
        {
            "img": "./wea_img/xiaoxue.png",
            "weather": "小雪"
        },
        {
             "img": "./wea_img/zhongxue.png",
            "weather": "中雪"
        },
        {
             "img": "./wea_img/daxue.png",
            "weather": "大雪"
        },
        {
            "img": "./wea_img/baoxue.png",
            "weather": "暴雪"
        },
        {
            "img": "./wea_img/wu.png",
            "weather": "雾"
        },
        {
          "img": "./wea_img/dongyu.png",
            "weather": "冻雨"
        },
        {
            "img": "./wea_img/shachenbao.png",
            "weather": "沙尘暴"
        },
        {
            "img": "./wea_img/fuchen.png",
             "weather": "浮尘"
        },
        {
           "img": "./wea_img/yangsha.png",
            "weather": "扬沙"
        },
        {
              "img": "./wea_img/qiangshachenbao.png",
            "weather": "强沙尘暴"
        },
        {
             "img": "./wea_img/mai.png",
            "weather": "霾"
        }
    ];
     
       //获取位置
    function getLocation(){
        if (navigator.geolocation){
            navigator.geolocation.getCurrentPosition(showPosition);
        }else{
            alert('不支持获取位置');
        }
    }
    //显示地图
    function showPosition(position){
       // 百度地图API功能
        var map = new BMap.Map("allmap");
        var point = new BMap.Point(position.coords.longitude,position.coords.latitude);
        map.centerAndZoom(point,12);
        var myCity = new BMap.LocalCity();
        myCity.get(myFun);
    }
     //获取市
    function myFun(result){
        var city = result.name; 
        if(city && city.indexOf('市') !=-1){
           city = city.replace(/市/, "");
        }else{
            city ="";
        }
        alert('city:'+city);
      getWea(city);
   
     }
    
    function getWea(cityName){
          //新浪接口  city=城市或者缩写，不输入自动识别城市
            $.getScript('http://php.weather.sina.com.cn/iframe/index/w_cl.php?code=js&day=0&city='+ cityName+'&dfc=1&charset=utf-8',function(a){
            var s="",r="",q="";
            for(s in window.SWther.w){
                q=SWther.w[s][0];
                r={city:s,date:SWther.add.now.split(" ")[0]||"",day_weather:q.s1,night_weather:q.s2,day_temp:q.t1,night_temp:q.t2,day_wind:q.p1,night_wind:q.p2};
                console.log(r);
            var default_tr=false;
                var default_img='./wea_img/default.png';
                    if(q.s1.indexOf('到') !=-1 && q.s1.indexOf('转') !=-1 ){  //有转有到 取转
                        var wea_one =q.s1.split('转'); 
                            q.s1 =wea_one[1]; 
                            console.log("转："+q.s1);    
                }else{
                    if(q.s1.indexOf('到') !=-1 || q.s1.indexOf('转') !=-1 ){ 
                                if(q.s1.indexOf('到') !=-1){  
                                        var wea_one =q.s1.split('到'); 
                                        q.s1 =wea_one[1]; 
                                        console.log("到："+q.s1);
                                }else{
                                    var wea_one =q.s1.split('转'); 
                                        q.s1 =wea_one[1]; 
                                        console.log("转："+q.s1);
                                }
                                
                    }
                }
            
                //匹配
                for(var i=0; i<dataList.length;i++){
                    if(dataList[i].weather.indexOf(q.s1) !=-1){
                        console.log(dataList[i].weather);
                        var img_url =dataList[i].img;
                        $('#imgs').attr('src',img_url);
                        default_tr =true ;
                        break;
                    }
                }
                //没有匹配
                if(!default_tr){
                $('#imgs').attr('src',default_img);
                }
                
                //温度
                var warm_html ="";
                if(r.day_temp>r.night_temp){
                    warm_html = r.night_temp+"~"+ r.day_temp;
                }else{
                    warm_html = r.day_temp+"~"+ r.night_temp;
                }
                $("#city").html(r.city);
                $("#warm").html(warm_html+"℃");
            }
        });
    }

    getLocation();
    </script>
</body>
</html>
```js
