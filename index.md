---
layout: default
overview: true
---
<style type="text/css">
  body,div,ul,li,a,img{margin: 0;padding: 0;}
  ul,li{list-style: none;}
  a{text-decoration: none;}
  
  .wrapper{position:relative;margin-left: 180px;margin-top: 20px;border: 2px black;}
  .banner{width:888px;height:566px;overflow:hidden;}
  .imgList{width:888px;height:566px;z-index: 10;}
  .imgList li{display: none;}
  .imgList .imgOn{display: inline;}
  .bg{position:absolute;bottom:0px;width: 400px;height: 40px;z-index:20;opacity: 0.4;filter:alpha(opacity=40);background: black;}
  .infoList{position: absolute;left: 10px;bottom: 10px;z-index: 20;}
  .infoList li{display: none;}
  .infoList .infoOn{display: inline;color: white;}
  .indexList{position:absolute;right: 300px;bottom:0px;z-index: 20;}
  .indexList li{float: left;margin-right: 5px;padding: 2px 4px;border: 2px grey;background: grey;cursor: pointer;}
  .indexList .indexOn{background: red;font-weight: bold;color: white;}
</style>
<section>
<div class="wrapper"><!-- 最外层部分 -->
    <div class="banner"><!-- 轮播部分 -->
      <ul class="imgList"><!-- 图片部分 -->
        <li class="imgOn"><a href="#"><img src="{{ site.img_url }}img/1.jpg" width="888px" height="666px" alt="aa"></a></li>
		<li><a href="#"><img src="{{ site.img_url }}/2.jpg" width="888px" height="666px" alt="bb"></a></li>
		<li><a href="#"><img src="{{ site.img_url }}/3.jpg" width="888px" height="666px" alt="cc"></a></li>
      </ul>
    <div class="bg"></div> <!-- 图片底部背景层部分-->
      <ul class="infoList"><!-- 图片左下角文字信息部分 -->
        <li class="infoOn">puss in boots1</li>
        <li>puss in boots2</li>
        <li>puss in boots3</li>
      </ul>
	  <ul class="indexList"><!-- 图片右下角序号部分 -->
        <li class="indexOn">1</li>
        <li>2</li>
        <li>3</li>
      </ul>
    </div>
 </div>
</section>
<script type="text/javascript">
  var curIndex = 0, //当前index
      imgArr = getElementsByClassName("imgList")[0].getElementsByTagName("li"), //获取图片组
      imgLen = imgArr.length,
      infoArr = getElementsByClassName("infoList")[0].getElementsByTagName("li"), //获取图片info组
      indexArr = getElementsByClassName("indexList")[0].getElementsByTagName("li"); //获取控制index组
     // 定时器自动变换2.5秒每次
  var autoChange = setInterval(function(){ 
    if(curIndex < imgLen -1){ 
      curIndex ++; 
    }else{ 
      curIndex = 0;
    }
    //调用变换处理函数
    changeTo(curIndex); 
  },2500);
  //调用添加事件处理
  addEvent();
 
  //给右下角的图片index添加事件处理
 function addEvent(){
  for(var i=0;i<imgLen;i++){ 
    //闭包防止作用域内活动对象item的影响
    (function(_i){ 
    //鼠标滑过则清除定时器，并作变换处理
    indexArr[_i].onmouseover = function(){ 
      clearTimeout(autoChange);
      changeTo(_i);
      curIndex = _i;
    };
    //鼠标滑出则重置定时器处理
    indexArr[_i].onmouseout = function(){ 
      autoChange = setInterval(function(){ 
      if(curIndex < imgLen -1){ 
        curIndex ++;
      }else{ 
        curIndex = 0;
      }
    //调用变换处理函数
      changeTo(curIndex); 
    },2500);
    };
     })(i);
  }
}
  //变换处理函数
  function changeTo(num){ 
    //设置image
    var curImg = getElementsByClassName("imgOn")[0];
    fadeOut(curImg); //淡出当前 image
    removeClass(curImg,"imgOn");
    addClass(imgArr[num],"imgOn");
    fadeIn(imgArr[num]); //淡入目标 image
    //设置image 的 info
    var curInfo = getElementsByClassName("infoOn")[0];
    removeClass(curInfo,"infoOn");
    addClass(infoArr[num],"infoOn");
    //设置image的控制下标 index
    var _curIndex = getElementsByClassName("indexOn")[0];
    removeClass(_curIndex,"indexOn");
    addClass(indexArr[num],"indexOn");
  }
 
    //设置透明度
  function setOpacity(elem,level){ 
    if(elem.filters){ 
      elem.style.filter = "alpha(opacity="+level+")";
    }else{ 
      elem.style.opacity = level / 100;
    }
  }
 
  //淡入处理函数
  function fadeIn(elem){ 
    setOpacity(elem,0); //初始全透明
    for(var i = 0;i<=20;i++){ //透明度改变 20 * 5 = 100
      (function(){ 
        var level = i * 5;  //透明度每次变化值
        setTimeout(function(){ 
          setOpacity(elem, level)
        },i*25); //i * 25 即为每次改变透明度的时间间隔，自行设定
      })(i);     //每次循环变化一次
    }
  }
 
    //淡出处理函数
  function fadeOut(elem){ 
    for(var i = 0;i<=20;i++){ //透明度改变 20 * 5 = 100
      (function(){ 
        var level = 100 - i * 5; //透明度每次变化值
        setTimeout(function(){ 
          setOpacity(elem, level)
        },i*25); //i * 25 即为每次改变透明度的时间间隔，自行设定
      })(i);     //每次循环变化一次
    }
  }
 
  //通过class获取节点
  function getElementsByClassName(className){ 
    var classArr = [];
    var tags = document.getElementsByTagName('*');
    for(var item in tags){ 
      if(tags[item].nodeType == 1){ 
        if(tags[item].getAttribute('class') == className){ 
          classArr.push(tags[item]);
        }
      }
    }
    return classArr; //返回
  }
 
  // 判断obj是否有此class
  function hasClass(obj,cls){  //class位于单词边界
    return obj.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'));
   }
   //给 obj添加class
  function addClass(obj,cls){ 
    if(!this.hasClass(obj,cls)){ 
       obj.className += cls;
    }
  }
  //移除obj对应的class
  function removeClass(obj,cls){ 
    if(hasClass(obj,cls)){ 
      var reg = new RegExp('(\\s|^)' + cls + '(\\s|$)');
         obj.className = obj.className.replace(reg,'');
    }
  }
</script>
