# day3 - Playing with CSS Variables and JS

1. 通过滑块拖动可以调整图片padding跟blur。
2. 通过调整color piker可以改变图片背景颜色以及字符串JS的颜色。

# 关键步骤

1. 使用:root声明全局CSS变量,通过--* 声明自定义变量。
```css
:root {
  --base: #ffc500;
  --spacing: 10px;
  --blur: 10px;
}
```

2. 在对应css使用css变量。
```css
img {
  padding: var(--spacing);
  background: var(--base);
  filter: blur(var(--blur));
}

.hl {
  color: var(--base);
}
```

3. 获取所有div.controls 下所有的input元素
```javascript
const inputs = document.querySelectorAll('.controls input');
```

4. 给每个元素绑定上监听拖动滑条/选调色板数值改变以及拖动滑条时的事件。
```javascript
inputs.forEach(input => input.addEventListener('change', handleUpdate));
    inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));
```
5. handleUpdate函数。
- 获取存放在dataset的单位后缀
- 改变目前操作的对应目标所对应的在全局CSS变量的值
```javascript
const suffix = this.dataset.sizing || '';
document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
```