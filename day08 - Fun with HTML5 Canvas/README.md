# day8 - Fun with HTML5 Canvas

使用canvas实现一个绘画板的功能。绘画过程中，画笔颜色会不断改变，宽度也在不断改变(方向为true宽度递增，方向为false宽度递减，取决于临界值)。

# 关键步骤

1. 创建一个canvas的上下文
```javascript
const canvas = document.querySelector('#draw');
const ctx = canvas.getContext('2d');
```

2. 初始化画布大小为window对象的大小
```javascript
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
```

3. 给canvas添加基本样式,其中，strokeStyle为线条描边的颜色，lineJoin为线条相交的方式，lineCap为笔触的形状，lineWidth为线条的(初始)宽度，globalCompositeOperation为合成操作的类型，比如设置为multiply是顶层与底层像素相乘，两种不同颜色的叠加在一起的时候颜色会发生变化。
```javascript
ctx.strokeStyle = '#BADA66';
ctx.lineJoin = 'round';
ctx.lineCap = 'round';
ctx.lineWidth = 100;
ctx.globalCompositeOperation = 'multiply';
```

4. 初始化部分判断条件以及位置的参数。
```javascript
// 是否正在绘画
let isDrawing = false;
// 最后停留的x坐标
let lastX = 0;
// 最后停留的y坐标
let lastY = 0;
// 初始色调的值,0 - 360
let hue = 0;
// 画笔宽度变化的方向,true为变大，false为变小
let direction = true;
```

5. 编写绘画动作函数。
```javascript
function draw(e) {
  // 鼠标在移动过程中是否执行(根据鼠标是否按下判断)绘画动作
  if (!isDrawing) {
    return;
  }
  console.log(e);
  // 每次绘画的时候改变色调
  ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
  // 开始绘画的路径
  ctx.beginPath();
  // 移动到点击的位置(位置在点击的时候初始化)
  ctx.moveTo(lastX, lastY);
  // 跟着鼠标移动连接
  ctx.lineTo(e.offsetX, e.offsetY);
  // 绘画
  ctx.stroke();
  // 把当前位置赋值给lastX跟lastY
  [lastX, lastY] = [e.offsetX, e.offsetY];
  // 每次绘画色调值递增
  hue++;
  // 色调值超过360归零
  if (hue >= 360) {
    hue = 0;
  }
  // 改变画笔width的变化放心啊
  if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
    direction = !direction;
  }
  // 根据方向去改变画笔宽度
  if (direction) {
    ctx.lineWidth++;
  } else {
    ctx.lineWidth--;
  }
}
```

6. 监听绘画事件
```javascript
// 鼠标按下的时候开始绘画事件，并且把当前按下的位置赋值给lastX跟lastY
canvas.addEventListener('mousedown', (e) => {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
});
// 鼠标按下并移动，触发绘画事件
canvas.addEventListener('mousemove', draw);
// 松开按下的鼠标按键/移出画布，停止绘画事件
canvas.addEventListener('mouseup', () => isDrawing = false);
canvas.addEventListener('mouseout', () => isDrawing = false);
```