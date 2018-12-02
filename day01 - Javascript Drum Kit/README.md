# day1 - 完成一个键盘鼓

1. 按下页面中对应键盘按键，会发出对应鼓的声音。
2. 成功按下对应按键后，会有相应过渡效果。

# 关键步骤

1. 通过document.addEventListener监听`按下任意按键`事件，并通过e.keyCode获取按下的按键对应的keyCode。
```javascript
document.addEventListener('keydown', (e) => {
  console.log(e.keyCode);
});
```

2. 通过querySelector获取元素，通过data-key属性作为选择器条件,获取按下的按键对应的audio标签。
```javascript
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
```

3. audio.currentTime = 0,设置每次按下对应按键的时候，确保从0开始播放该音频;audio.play(),播放该音频。

4. Selector.classList.add等价于jQuery的Selector.addClass

5. 获取所有key标签，并监听他们的css Transition 结束并触发移除playing class的事件。
```javascript
const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition))
```