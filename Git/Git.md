# Git 版本回退

1.  查询历史对应不同版本的ID ，用于回退使用。
```js
$ git log --pretty=oneline
```

2. 恢复到历史版本
```js
$ git reset --hard fae6966548e3ae76cfa7f38a461c438cf75ba965
```

3. 把修改推送到服务器
```js
$ git push -f -u origin master  
```