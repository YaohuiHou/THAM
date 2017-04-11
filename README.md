# THAW
卡车之家weex开发--native与js通信



---
>     THAW 模块名 （truckhome and weex）在此模块下进行扩展和补充。
---

- [返回按钮](#返回按钮)
- [主动获取客户端信息](#主动获取客户端信息)
- [拨打电话](#拨打电话)
- [拍照或从手机相册中选图接口](#拍照或从手机相册中选图接口)
- [监听上传图片回调](#监听上传图片回调)
- [获取经纬度](#获取经纬度)
- [根据经纬度显示地图](#根据经纬度显示地图)
- [判断是否登陆](#判断是否登陆)
- [去登陆](#去登陆)
- [登陆回调](#登陆回调)
- [地理位置提交](#地理位置提交)
- [分享](#分享)
- [显示正在加载弹层](#显示正在加载弹层)
- [隐藏正在加载弹层](#隐藏正在加载弹层)
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

# 监听上传图片回调
用户上传图片的时候监听是否上传成功，返回JSON：{imageUpload:'图片预览地址'}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
let globalEvent = weex.requireModule('globalEvent');
<script>
    export default {
        call (){
            //   native操作
            globalEvent.addEventListener('chooseImageCallBack',function(res){

            });
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


# 登陆回调
点击去登陆的时候监听用户登陆是否成功，如果成功返回用户的userId 返回JSON：{userId:''}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
let globalEvent = weex.requireModule('globalEvent');
<script>
    export default {
        call (){
            //   native操作
            globalEvent.addEvenetListener('onGoLoginCallBack',function(data){
                
            })
        }
    }
</script>
```

# 地理位置提交
点击按钮返回用户选择的经纬度和街道名称 返回JSON：{longitude:'',latitude:'',address:''}

```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
let globalEvent = weex.requireModule('globalEvent');
<script>
    export default {
        call (){
            //   native操作
            globalEvent.addEventListener('onLocationCommit',function(res){

            });
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


# 显示正在加载弹层
    接受一个string参数，正在加载的文案显示为所传的参数
```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>let
     let thaw = weex.requireModule('THAW')
     export default {
        call (){
            //   native操作
            thaw.onShowLoading('正在加载中...');
        }
    }
</script>
```

# 隐藏正在加载弹层
    隐藏正在加载的弹层
```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>let
     let thaw = weex.requireModule('THAW')
     export default {
        call (){
            //   native操作
            thaw.onHideLoading();
        }
    }
</script>
```

# 响应客户端返回按键
    直接返回到app页面
```
<!--html-->
<template>
    <div @click="call" class="phone"></div>
</template>

<!--js-->
<script>let
     let thaw = weex.requireModule('THAW')
     export default {
        call (){
            //   native操作
            thaw.onGoBack();
        }
    }
</script>
```

# 如果请求失败，返回格式
JSON：{state:error,data,"失败原因"}
