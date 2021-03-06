# DragDismissLayout

## 关于

仿微信朋友圈浏览大图下拉返回交互。

看到这个功能的时候，第一感觉就是比较人性化，简化了大屏手机关闭浏览页的操作，防止大屏手机上面的误操作。github上面还没搜到相关的库，就自己造了个轮子。效果如下:

![image](https://github.com/yuanyang1991/DragDissmissLayout/blob/master/wechat_image.gif)

## 使用以及介绍

### 依赖
在项目级的build.gradle下面添加：
```
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
    }
}

```

最新版本：[![Release](https://jitpack.io/v/yuanyang1991/DragDismissLayout.svg)](https://jitpack.io/#yuanyang1991/DragDismissLayout)

然后在module级的build.gradle下面添加：
```
compile 'com.github.yuanyang1991:DragDismissLayout:最新版本'
```






### 使用

设置Activity的主题位透明模式：(一般设置位全屏模式)

```

 <style name="AppTheme.FullScreen" parent="@style/Theme.AppCompat.NoActionBar">
    <item name="android:windowBackground">@android:color/transparent</item>
    <item name="android:windowIsTranslucent">true</item>
    <item name="android:windowContentOverlay">@null</item>
    <item name="colorPrimary">@color/fm_toolbar_color</item>
    <item name="colorPrimaryDark">@color/fm_statusbar_color</item>
  </style>

```


在布局文件
```
<?xml version="1.0" encoding="utf-8"?>
<com.example.lib.DragDismissLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/dragLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.yuanyang.dragdismisslayout.ImageDisplayActivity">

    <OtherView
        android:id="@+id/photoView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</com.example.lib.DragDismissLayout>

```

DragDismissLayout需要知道前一个页面ImageView的x,y坐标，以及长和宽
```
//通过LocationUtil.getRawLocation获取目标View的绝对坐标以及长宽
    public static void show(View view,String imageUrl ,Context context){
        Location location = LocationUtil.getRawLocation(view);
        show(location.x,location.y,view.getMeasuredWidth(),view.getMeasuredHeight(),imageUrl,context);
    }

    public static void show(int x, int y, int width, int height, String imageUrl,Context context){
        Intent intent = new Intent(context,ImageDisplayActivity.class);
        intent.putExtra("x",x);
        intent.putExtra("url",imageUrl);
        intent.putExtra("y",y);
        intent.putExtra("width",width);
        intent.putExtra("height",height);
        context.startActivity(intent);
    }


//设置数据到DragDissmissLayout中
pullDownLayout.setTargetData(x,y,width,height);

```


### 已知问题
1. 透明背景会造成Activity生命周期的调用顺序发生改变，可能由此导致一些问题，目前发现在透明主题的页面使用QQ分享会发生UI问题。其他分享暂无。



### 原理
1. 通过onInterceptTouchEvent判断拦截触摸事件，有的浏览大图控件有自己的交互，比如PhotoView

2. 通过onTouchEvent消费事件。处理上滑下滑，临界点判断逻辑。

3. 获取View在屏幕上的绝对坐标

4. 滑动处理，Scroller，offsetTopAndBottom...

5. 具体见代码



