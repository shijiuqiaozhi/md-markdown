# 0.canvas特征

1.canvas是基于"位图"的，放大后会失真，而svg是基于"矢量"的，

放大后不会失真。

canvas的坐标系和数学上的坐标系不同，数学上的坐标系是y轴向上的，而canvas的坐标轴是y轴向下的。



# 一.直线图形

## 1.画线

先。定义一个canvas

```
<canvas 
    id="canvas" 
    width="200" 
    height="150"
    style="border: 1px dashed gray;"
  ></canvas>
```

id方便用document.getElementById来获取，width和height属性

是canvas画布的宽高。<strong>这里推荐还是用属性上定义canvas的宽高而不是css。因为用css定义的宽高会有偏差</strong>

然后就是三部曲

先获取canvas对象，再获取上下文对象context。然后利用context的各种属性来绘制图形。

```
//1. 获取canvas对象
  let cnv = document.getElementById('canvas');
  //2. 获取上下文对象context
  let cxt = cnv.getContext('2d');
  //3. 开始绘制图形,利用context的各种属性
  // moveTo(x1, y1)，画笔移动到坐标
  cxt.moveTo(50, 100);
  // lineTo(x2, y2),画笔从起点画线画到(x2, y2)
  cxt.lineTo(150, 50);
  // 现在会从(150, 50)画直线到(50, 0)
  cxt.lineTo(50, 0);
  // 可以用moveTo去重置画笔初始位置。如果不想从(150, 50)为起点的话
  cxt.lineTo(50, 100);
  cxt.stroke(); 
```

moveTo(x1, y1)指画笔移动到(x1, y1)坐标点，stroke()指停止绘画

## 2.画矩形

### 1.三种方法，strokeRect,fillRect和rect()

虽然理论上可以通过moveTo和lineTo来画各种图形。但是矩形有自己的api可供方便画出

```
// strokeStyle属性和 strokeRect方法，
  
  cxt.strokeStyle = 'red';
  // 分别是矩形左上角坐标(50, 50),宽和高80，
  // 不过这种画法出来的是描边矩形
  cxt.strokeRect(50, 50, 80, 80);
  // 这两个api画出来是填充矩形，但是没有边
  cxt.fillStyle = 'blue';
  cxt.fillRect(50, 50, 80, 80);

  // 可以同时使用两个api，就可以画一个有边的填充矩形
```

还有rect()方法。和上述矩形api参数相同，不过区别是strokeRect()

和fillRect()这两个方法会在调用后直接把矩形绘制处理啊，而rect()方法只有在使用后再调用stroke()或fill()方法才会绘制出矩形。

```
cxt.strkeStyle = 'red';
cxt.rect(50, 50, 80, 80);
cxt.stroke();
```

等价于

```
cxt.strokeStyle = 'red';
cxt.strokeRect(50, 50, 80, 80);
```

同理也有

```
cxt.fillStyle = 'red';
cxt.rect(50, 50, 80, 80);
cxt.fill()
```

### 2.清空矩形 clearRect()

可以用clearRect()清空指定矩形区域

cxt.clearRect(x, y, width, height);

x, y 分别表示矩形区域最左上角的坐标，width表示矩形的宽度，

height表示矩形的高度。

例如若cxt.fillRect(50, 50, 80, 80); cxt.clearRect(60, 60, 50, 50),则

会出现![image-20220610172500412](C:\Users\16448\AppData\Roaming\Typora\typora-user-images\image-20220610172500412.png)

## 3.画正多边形

```javascript
createPolygon(cxt, 3, 100, 75, 50);
  cxt.fillStyle = "HotPink";
  cxt.fill();
  /* n: 表示n边形
   * dx, dy: 表示n边形中心坐标
   * size: 表示n边形的大小
  */
  function createPolygon(cxt, n, dx, dy, size) {
    cxt.beginPath();
    let degree = (2 * Math.PI )/ n;
    for ( let i = 0; i < n; i++) {
      let x = Math.cos(i * degree);
      let y = Math.sin(i * degree);
      cxt.lineTo(x * size + dx, y * size + dy);
    }
    cxt.closePath();
  }
```

牵扯到一些数学==

## 4.画五角星

```
createPentagon();
  cxt.fillStyle = "HotPink";
  cxt.fill();
  function createPentagon() {
    cxt.beginPath();
    for (let i = 0; i < 5; i++) {
      cxt.lineTo
        (
          Math.cos
          ((18 + i * 72) * Math.PI / 180) * 50 + 100,
          -Math.sin
          ((18 + i * 72) * Math.PI / 180) * 50 + 100              
        );
      cxt.lineTo
        (
          Math.cos
          ((54 + i * 72) * Math.PI / 180) * 25 + 100,
          -Math.sin
          ((54 + i * 72) * Math.PI / 180) * 25 + 100              
        );
    }
    cxt.closePath();
    cxt.stroke();
  }
```

## 5.画圆形

```javascript
/*
    *x, y 表示圆心坐标
    *开始角度和结束角度都以"弧度"为单位,
    例如180度就应写成Math.PI,360即为Math.PI*2
    (关于开始角度和结束角度，实际开发中一般这样写
    : 度数 * Math.PI/180,例 120*Math.PI/180 // 120度)
    *anticlockwise是一个布尔值,为true时逆时针，false相反
    ,默认值为false。
    *此api仅是对圆形的状态描述，后续还要对圆形进行
    描边和填充，这点同绘制矩形。
  */
  cxt.arc(x, y, 半径, 开始角度, 结束角度, anticlockwise);
  cxt.closePath();
  // 描边
  cxt.strokeStyle = "颜色值";
  cxt.stroke();
  // strokeStyle用于定义边框颜色，stroke用于描边动作。
  // fillStyle用于定义填充的颜色，fill用于填充动作，使用同
stroke那一套
```

画描边圆: 开始路径，arc()画圆,结束路径,描边操作