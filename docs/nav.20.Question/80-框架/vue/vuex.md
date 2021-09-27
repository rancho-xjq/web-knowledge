1. 使用vuex遇到过什么问题，什么方法解决的
…语法
扩展运算符spread，在mapState中的作用： 首先要搞清mapState的作用，mapState的返回值是一个对象，对象里有方法，用来获取state的数据属性，computed接受这个返回值。

``` js
computed:{
  a(){
    return this.b + this.c
  },
  mapState({
    xxx(){
      return this.$store.state['xxx']
    }
  })
}
  // 如果光像上面这么写，computed变成了：
  // computed:{
  //   a(){return this.b + this.c},
  //   {
  //     xxx(){return this.$store.state['xxx']}
  //   }
  // }
  // 这样是不行的，因为xxx不是computed的直接子属性，不引入扩展运算符...的话，只能像下面这么写
computed:mapState({
  // 1.先把原来的复制进来
   a(){
    return this.b + this.c
  },
  // 2. 引入需要的state数据对象
  xxx(){
    return this.$store.state['xxx']
  }
})
每次这样写都会很麻烦， 这时候扩展运算符...万丈光芒地走来了，它的作用就是将当前对象中的属性融入到外部对象中

var asMapState = {x: 1, y: 2}
var asComputed = {q: 6, w: 7, ...asMapState}
// asComputed:{q: 6, w: 7, x: 1, y: 2}
```
2. vuex是做什么的？缺点？

3. action 与 mutation 的区别
mutation 是同步更新， $watch 严格模式下会报错
action 是异步操作，可以获取数据后调用 mutation 提交最终数据