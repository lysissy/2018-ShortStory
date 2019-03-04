# CSS3 Gradient

> css3添加了很多视觉交互的属性方法，经常用的有：
> * transform提供了图形变换相关的方法
> * gradient提供了颜色渐变的方法。
> * animation提供了类似逐帧动画的效果
> * keyframe关键帧提供了类似补间动画的效果。

### 渐变的种类

* 线性渐变 linear-gradient
* 径向渐变 radial-gradient
* 重复渐变 repeat-linear-gradient

### 渐变的方法参数
 
> 省略，自行查手册。几个关键词的含义列举如下：

|关键词              |含义     |
|---------          |--------|
|closest-side	    |渐变的边缘形状与容器距离渐变中心点最近的一边相切（圆形）或者至少与距离渐变中心点最近的垂直和水平边相切（椭圆）。
|closest-corner	    |渐变的边缘形状与容器距离渐变中心点最近的一个角相交。
|farthest-side	    |与closest-side相反，边缘形状与容器距离渐变中心点最远的一边相切（或最远的垂直和水平边）。
|farthest-corner	|渐变的边缘形状与容器距离渐变中心点最远的一个角相交。

### 渐变的实现原理

参见参考资料1。了解原理才能更好的解决问题，而不是盲目地试误。

### 渐变的使用

#### 滤镜效果
背景是由第一个指定的背景在最上面（可以做遮罩使用）, 然后接下来的背景层叠起来。
```txt
background-image: linear-gradient(to right, rgba(255,255,255,0), rgba(255,255,255,1)), url(http://foo.com/image.jpg);
```
以上代码可以实现一张背景图片上类似滤镜的效果。<br>
![滤镜](https://mdn.mozillademos.org/files/4275/linear_multibg_transparent2.png)
#### 复杂背景
```txt
background-image: repeating-linear-gradient(90deg, transparent, transparent 50px,
      rgba(255, 127, 0, 0.25) 50px, rgba(255, 127, 0, 0.25) 56px, transparent 56px, transparent 63px,
      rgba(255, 127, 0, 0.25) 63px, rgba(255, 127, 0, 0.25) 69px, transparent 69px, transparent 116px,
      rgba(255, 206, 0, 0.25) 116px, rgba(255, 206, 0, 0.25) 166px),
repeating-linear-gradient(0deg, transparent, transparent 50px, rgba(255, 127, 0, 0.25) 50px,
      rgba(255, 127, 0, 0.25) 56px, transparent 56px, transparent 63px, rgba(255, 127, 0, 0.25) 63px,
      rgba(255, 127, 0, 0.25) 69px, transparent 69px, transparent 116px, rgba(255, 206, 0, 0.25) 116px,
      rgba(255, 206, 0, 0.25) 166px),
repeating-linear-gradient(-45deg, transparent, transparent 5px, rgba(143, 77, 63, 0.25) 5px,
      rgba(143, 77, 63, 0.25) 10px),
repeating-linear-gradient(45deg, transparent, transparent 5px, rgba(143, 77, 63, 0.25) 5px,
      rgba(143, 77, 63, 0.25) 10px);
```
![复杂渐变](https://mdn.mozillademos.org/files/3682/repeat_background_gradient_checked.png)


###### 参考资料
1. [CSS Gradient详解 | AlloyTeam](http://www.alloyteam.com/2016/03/css-gradient/)
2. [使用CSS渐变 - Web开发者指南 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Using_CSS_gradients)
3. [radial-gradient() - CSS | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient)
