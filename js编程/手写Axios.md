### 手写Axios

```js
    var Axios = {
      gat: function (url) {
        return new Promise((resolve, rejext) => {
          var xhr = XMLHttpRequest();
          xhr.open('GET', url.true);
          xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && xhr.status == 200) {
              resolve(xhr.responseText)
            }
          }
          xhr.send();
        })
      }
    }
```