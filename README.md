# weather
实时全国天气 ，自动定位城市，JavaScript

#![Alt text](https://github.com/chenyufeng1991/NewsClient/raw/master/Screenshots/2.png)

--javascript
  //新浪接口  city=城市或者缩写，不输入自动识别城市
    $.getScript('http://php.weather.sina.com.cn/iframe/index/w_cl.php?code=js&day=0&city=&dfc=1&charset=utf-8',function(a){
    var s="",r="",q="";
    for(s in window.SWther.w){
        q=SWther.w[s][0];
        console.log(q);
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
--javascript
