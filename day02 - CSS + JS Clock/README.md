# day2 - CSS + JS Clock 

1. 使得时钟指针拥有过渡效果(CSS过渡效果)
2. 通过JavaScript去改变时钟对应时针旋转度数。

# 关键步骤

1. 使用transform-origin改变指针的旋转中心。

```css
.hand {
  transform-origin: 100%;
}
```

2. 改变指针初始位置为12点钟方向，需要把指针旋转90度。

```css
.hand {
  transform: rotate(90deg);
}
```

3. 给指针旋转添加过渡效果, 其中三次贝塞尔曲线函数用于给指针模拟滴答过程中的回弹效果。

```css
.hand {
  transition: all 0.05s;
  transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);
}
```

4. 使用setInterval定时执行任务，用于每秒钟触发一次对应的函数用于改变指针状态。

```javascript
  setInterval(setDate, 1000);
```

5. setDate函数主要组成部分


- 时间 new Date()

```javascript
const date = new Date();
```

- 获取当前时、分、秒

```javascript
const hours = date.getHours();
const mins = date.getMinutes();
const seconds = date.getSeconds();
```

- 计算时、分、秒针旋转度数，分针跟秒针计算一样，而时针比较特殊，因为在实际中，时针会随着分针偏转而偏转，在基础上还要加上当前分针偏转比例对应的度数。由于hand的css默认旋转90度，所以三者都要加上90度。

```javascript
const hourDegrees = ((hours / 12) * 360) + ((mins / 60) * (1 / 12) * 360) + 90;
const minsDegrees = (((mins / 60)) * 360) + 90;
const secondsDegrees = (((seconds / 60)) * 360) + 90;
```

- 最后，给transform的rotate属性赋值，用于指定对应指针的旋转度数。

```javascript
hourHand.style.transform = `rotate(${hourDegrees}deg)`;
minHand.style.transform = `rotate(${minsDegrees}deg)`;
secondHand.style.transform = `rotate(${secondsDegrees}deg)`;
```
