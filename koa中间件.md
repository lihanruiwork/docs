# KOA中间件
使用中间件
```javascript
app
  .use(logger())
  .use(bodyParser())
  .use(helmet())
```
* use：将调用的中间件添加到this.middleware数组中
* 中间件写法
```javascript
const one = (ctx, next) => {
  console.log('>> one');
  await next();
  console.log('<< one');
}
```
* compose：将中间件级联执行
```javascript
function compose(middleware){
  Array.isArray() //判断是否是数组 
  for(const fn of middleware){typeof fn == ‘function’} //判断是否是函数 
  return function(context, next) {
    index = -1   //记录上一次执行的中间件的位置
    
    return dispatch(0);
    
    function dispatch(i){
      if(i <= index) reject  
      index=i
      fn = middleware[i]
      if(i === length) fn=next
      if(!fn) return Promise.resolve
      return Promise.resolve(fn(context,function next() {
	      return dispatch(i+1)
      }))
    }
  }
}
```
* 每个中间件都是一个闭包，同一个中间件i是不变的
* 每个中间件只能执行一次
    * 如果i <= index说明next方法调用多次
* 直到最后一个中间件执行完，返回Promise，倒数第二个中间件才执行后续代码并返回Promise
* 一直以这种方式执行直到第一个中间件执行完，因此是洋葱模型
