# HTML 拖曳教學範例

- dataTransfer 似乎不能放 Instance or Element Node。可能考慮設定一個 Global/Scope 變數來放！
- drag/drop Event doc ref: https://developer.mozilla.org/zh-TW/docs/Web/Guide/HTML/Drag_and_drop

# 拖曳物件目前已知的事件觸發流程

- 觸發標的會 a, b 交錯！ ex: sample04.html

~~~
a.被拖曳物件的Node
b.可以被放置的Node

a must be draggable="true"
a is ondragstart
b is ondragenter
b is ondragover (滑鼠移動連續且持續觸發！)
b is ondragleave
b is ondrop (當滑鼠放開的時候必須在可以 drop 並且有間聽的Node物件上才會觸發！必須下 e.preventDefault() 關閉預設行為！)
a is ondragend (緊接著 ondrop 後觸發)

* 補充：HTML Element Node 是可以堆疊的有時候事件的觸發 e.target 會是該 Node 的 Child！
~~~

~~~
var dragSrcEl = null;

function handleDragStart(e) {
  // Target (this) element is the source node.
  this.style.opacity = '0.4';

  dragSrcEl = this;

  e.dataTransfer.effectAllowed = 'move';
  e.dataTransfer.setData('text/html', this.innerHTML);
}

function handleDrop(e) {
  // this/e.target is current target element.

  if (e.stopPropagation) {
    e.stopPropagation(); // Stops some browsers from redirecting.
  }

  // Don't do anything if dropping the same column we're dragging.
  if (dragSrcEl != this) {
    // Set the source column's HTML to the HTML of the column we dropped on.
    dragSrcEl.innerHTML = this.innerHTML;
    this.innerHTML = e.dataTransfer.getData('text/html');
  }

  return false;
}
~~~
