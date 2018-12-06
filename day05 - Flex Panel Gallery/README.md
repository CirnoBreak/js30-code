# day5 - Flex Panel Gallery

1. 使用CSS + JS实现一个可伸缩的画廊。点击对应图片，会有展开跟收缩的动画过度效果。
2. 过程:
  - 图片展开: 点击图片(调用toggleOpen) -> 图片展开(添加.open) -> 等待过渡效果结束(transitionend,调用toggleActive) -> 在视图外的文字往中心移动(添加.open-active)
  - 图片收缩: 点击图片(调用toggleOpen) -> 图片收缩(移除.open) -> 等待过渡效果结束(transitionend,调用toggleActive) -> 在试图内的文字往外移动(移除.open-active)

# 关键步骤

1. 把.panels(画廊的容器)设置为display: flex
```css
.panels {
  display: flex;
}
```

1. 把.panel(.panels里的子元素)的flex改成1，使其大小平均，然后设置display: flex;让其内部形成弹性盒子。justify-content跟align-items为center使得它水平垂直居中，并设置flex-direction: column;把弹性盒子主轴改成垂直方向(文字)。
```css
.panel {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
```

3. 给.panel下所有的子元素(三个垂直排列的文字)添加过渡效果，用于呈现展开/收缩图片过渡效果结束后的文字过渡。并让他们大小平均并水平垂直居中。其中flex的三个参数为:
  - flex-growth 弹性盒子项(flex item)的拉伸因子
  - flex-shrink 指定了flex元素的收缩规则
  - flex-basis 指定了flex元素在主轴方向的初始大小
  - flex 规定了弹性元素如何伸长或缩短以适应flex容器中的可用空间，以上三个的简写
```css
.panel > * {
  transition: transfrom 0.5s;
  flex: 1 0 auto;
  justify-content: center;
  align-items: center;
}
```

4. 把.panel里的第一跟最后一个文字的位置移到视图外(收缩状态)
```css
.panel > *:first-child {
  transform: translateY(-100%);
}
.panel > *:last-child {
  transform: translateY(100%);
}
```

5. 把.panel里第一跟最后一个文字的位置移到视图内(展开状态)
```css
.panel.open-active > *:first-child {
  transform: translateY(0);
}
.panel.open-active > *:last-child {
  transform: translateY(0);
}
```

6. 获取panel元素的列表，绑定点击事件，并触发toggleOpen
```javascript
const panels = document.querySelectorAll('.panel');
function toggleOpen(e) {
    this.classList.toggle('open');
}
panels.forEach( panel => panel.addEventListener('click', toggleOpen);
```

7. 绑定transitionend事件，并触发toggleActive
```javascript
function toggleActive(e) {
  if(e.propertyName.includes('flex')) {
    this.classList.toggle('open-active');
  }
}

panels.forEach(panel => panel.addEventListener('transitionend', toggleActive));
```