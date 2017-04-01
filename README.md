# THAW
卡车之家weex开发--native与js通信



---
>     THAW 模块名 （truckhome and weex）在此模块下进行扩展和补充。
---

- [返回按钮](#返回按钮)
- [主动获取客户端信息](#主动获取客户端信息)
- [拨打电话](#拨打电话)
- [拍照或从手机相册中选图接口](#拍照或从手机相册中选图接口)
- [获取经纬度](#获取经纬度)
- [根据经纬度显示地图](#根据经纬度显示地图)
- [判断是否登陆](#判断是否登陆)
- [去登陆](#去登陆)
- [分享](#分享)
- [如果请求失败，返回格式](#如果请求失败，返回格式)

# **返回按钮**

    文档位置：https://weex-project.io/cn/references/advanced/extend-to-android.html



```
<!-- weex -->
<template>
  <div>
    <div class="header">
      <div class="back" @click="back">
        <div class="icon"></div>
      </div>
    </div>
</template>

<script>
  export default {
    methods:{
      back:function(){
        //  判断是否为路由跳转
        if (this.$route && this.$route.params.id) {
          this.$router.push('/')
          return ;
        }
        //   native操作
        weex.requireModule('thaw').onGoBack("1");
        return ;
      }
    }
  }
</script>
```
*点击事件执行的时候给客户端传一个定好的值‘1’，告诉客户端需要做的操作*


# **主动获取客户端信息**


 在客户端打开页面的时候主动向native端传值1，module回调，通过onGetData方法通知，将得到的 信息进行操作




```
<script>
    export default {
        created () {
            var me = this;
            var thaw = weex.requireModule('thaw');
            thaw.onGetData('1',function(ret) {  
                me.userid = ret.userid;
                me.userName = ret.userName;
                //执行操作
            }
        }
    }
</script>
```

*这里是进入每个页面后得到用户信息，格式json，{'userid':'123456','userName':'牛逼啦123'}*          
*这个操作在created中执行，且this必须在外定义，否则找不到*


# 拨打电话
拨打电话和第一个点击返回按钮是一样的意思，这次用的是onGoCall方法，将电话号码传给调取电话功能。


```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('thaw').onGoCall("400-800-1616");
        }
    }
</script>
```
```

```
# 拍照或从手机相册中选图接口
点击按钮弹出询问用户调取摄像头还是图库，传递"album",代表只调取图库，如果传递 "camera"，代表只调取摄像头，默认两者都有。返回图片的预览地址
返回JSON：{state:"success":data:'预览地址'}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').chooseImage();
        }
    }
</script>
```


# 获取经纬度
获取用户当前设备所在位置的经纬度。
返回JSON：{state:"success":data{latitude:'',longitude:''}}


```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').getLocation();
        }
    }
</script>
```


# 根据经纬度显示地图
发送经纬度调取地图，标注位置点始终在所发送的经纬度，如果有第三个参数，第三个参数为"auto",那么标注位置始终为地图的中心点，并返回地图中心点的经纬度和地址名称
返回JSON：{state:"success":data{latitude:'',longitude:'',name:''}}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').onShowMap({latitude : ' ',longitude : ' ',type:'auto'});
        }
    }
</script>
```

# 判断是否登陆
判断用户是否登陆，登陆返回用户的id，未登录返回为0，返回JSON：{state:'success',data:'0'}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').onIsLogin();
        }
    }
</script>
```

# 去登陆
调起登陆弹层

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').onGoLogin();
        }
    }
</script>
```


# 分享

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>
    export default {
        call (){
            //   native操作
            weex.requireModule('THAW').onMenuShare({
                title: "", // 分享标题
                desc: "", // 分享描述
                link: "", // 分享链接
                imgUrl: "" // 分享图标
            });
        }
    }
</script>
```

# 如果请求失败，返回格式
JSON：{state:error,data,"失败原因"}
