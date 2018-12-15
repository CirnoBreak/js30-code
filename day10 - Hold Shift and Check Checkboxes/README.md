# day10 - Hold Shift and Check Checkboxes

实现一个类似windows按住shift可以多选checkbox的功能，并且选中的原速中间有删除线。

关键步骤:
1. 获取所有checkbox
```javascript
const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]');
```

2. 声明最后一次选中checkbox的变量lastChecked。
```javascript
let lastChecked;
```

3. 编写选中事件handleCheck
```javascript
function handleCheck (e) {
  // 用于判断是否在按下shift的跟最后一个勾选的中间范围
  let inBetween = false;
  // 检查是否按下shift键并检查有没有选中
  if (e.shiftKey && this.checked) {
    checkboxes.forEach(checkbox => {
      console.log(checkbox);
      console.log(inBetween)
      // 遍历到两端元素的时候取反，因为有可能是从上到下或者从下到上
      if (checkbox === this || checkbox === lastChecked) {
        inBetween = !inBetween;
      }
      // 在中间范围，则设置为选中
      if (inBetween) {
        checkbox.checked = true
      }
    });
  }
  // 把最后一次选中的设置为当前点击的checkbox
  lastChecked = this;
}
```
4. 监听点击事件
```javascript
checkboxes.forEach(checkbox => checkbox.addEventListener('click', handleCheck));
```
