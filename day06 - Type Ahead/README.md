# day6 - Type Ahead

实现一个搜索功能，可以根据输入内容查找到对应的信息，并把在输入框输入匹配的关键词在渲染的时候高亮。

# 关键步骤

1. 通过fetch api获取对应的数据,并存放到cities数组里。
```javascript
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';
let cities = [];
// 获取数据
fetch(endpoint)
  .then(blob => blob.json())
  .then(data => cities.push(...data))
```

2. 编写从cities数组里面筛选出跟输入关键词匹配的数据的函数findMatches。
```javascript
function findMatches(wordToMatch, cities) {
  const regex = new RegExp(wordToMatch, 'gi');
  return cities.filter(place => {
    return place.city.match(regex) || place.state.match(regex);
  })
}
```

3. 编写把数字转成千位计数的字符串的函数findMatches。
```javascript
function numberWithComas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}
```

4. 编写渲染html的函数displayMatches，其中高亮的实现为:找到匹配的文字，替换为带有高亮class的span,并且其innerText为搜索框输入的内容。
```javascript
const suggestions = document.querySelector('.suggestions');
function displayMatches() {
  const matchArray = findMatches(this.value, cities);
  const html = matchArray.map(place => {
    const regex = new RegExp(this.value, 'gi');
    // 把匹配的城市名/州名中与输入字符串匹配的部分高亮
    const cityName = place.city.replace(regex, `<span class="hl">${this.value}</span>`)
    const stateName = place.state.replace(regex, `<span class="hl">${this.value}</span>`)
    return `
      <li>
        <span class="name">${cityName}, ${stateName}</span>
        <span class="population">${numberWithComas(place.population)}</span>
      </li>
    `
  }).join('');
  suggestions.innerHTML = html;
}
```

5. 获取元素并监听变动。
```javascript
const searchInput = document.querySelector('.search');

searchInput.addEventListener('change', displayMatches);
searchInput.addEventListener('keyup', displayMatches);
</script>
```