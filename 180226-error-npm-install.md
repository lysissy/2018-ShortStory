### 描述
运行
```npm
npm install xx
```
出现错误提示
```
Unexpected end of JSON input while parsing near '...n-precompile-charcode'
```

### 解决
运行
```
npm cache clean --force
```