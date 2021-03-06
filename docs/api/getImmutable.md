# `getImmutable` 方法

### 描述
`getImmutable`用于在当前Pipe节点下的得到一个immutable的store，用于可控制与对比前后值更新与判断对比，例如用于React PureComponent或者shouldComponentUpdate。


### 用法
```javascript
getImmutable(store)
```

### 参数
store(Object/Array): 当前store

### 返回值
(*): 返回当前immutable的store

### 示例
```javascript
import iFlow,{ getImmutable } from 'iflow'
import flow from 'react-iflow'

const pipe = iFlow({
  add(){
    this.counter += 1
  },
  counter: 0,
  foo: {
    bar: 88
  },
})

const store = pipe.create()

@flow(store)
class Foobar extends Component {
  render(){
    console.log(this.props.store)
    return (
      <Sub store={getImmutable(this.props.store.foo)}/>
    )
  }
}

class Sub extends Component {
  shouldComponentUpdate({store}) {
      return this.props.store !== store
  }
  render(){
    return <span>{this.props.store.bar}</span>
  }
}
```
