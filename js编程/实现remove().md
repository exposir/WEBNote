### 封装reomve()
通过removeChild()实现一个remove()

```js
        Element.prototype.removeElement = function () {
            this.parentElement.removeChild(this);
        }
        NodeList.prototype.removeElement = HTMLCollection.prototype.removeElement = function () {
            for (var i = this.length - 1; i >= 0; i--) {
                if (this[i] && this[i].parentElement) {
                    this[i].parentElement.removeChild(this[i]);
                }
            }
        }


        var box = document.getElementById("box");
        box.removeElement();
```