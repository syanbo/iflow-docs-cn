# 示例

本大章讨论到的可撤销/重做的TODO完整代码如下：

```javascript
import iFlow, { getState, setState } from 'iflow'

const pipe = iFlow({
  todo: [],
  tabStatus: 'All',
  tabs: [
    'All',
    'Active',
    'Completed'
  ],
  history: [{
    list: [],
    tabStatus: 'All'
  }],
  index: 1,
  add (text) {
    this.list.push({
      id: +new Date(),
      text,
      completed: false,
    })
  },
  toggleTodo (currentId) {
    const current = this.list.find(({id}) => id === currentId)
    current.completed = !current.completed
  },

  clearCompleted () {
    this.list = this.list.filter(({completed}) => !completed)
  },

  toggleTab (tabStatus) {
    this.tabStatus = tabStatus
  },
  onSubmit (e, input) {
    e.preventDefault()
    if (!!input.value.trim()) {
      this.add(input.value)
      input.value = ''
    }
  },
  record (actionName) {
    if ([
        'add',
        'toggleTodo',
        'clearCompleted',
      ].includes(actionName)) {
      const {
        list,
      } = getState(this)
      this.history.splice(this.index, this.history.length - this.index, {
        list,
      })
      this.index += 1
    }
  },
  doing (index) {
    this.index += index
    const {
      list,
    } = getState(this.history[this.index - 1])
    setState(this, {
      list,
    })
  }
}).addListener((...args) => {
    const [actionName] = args.slice(-2, -1)
    actionName !== 'record' && store.record(actionName)
  })

const store = pipe.create()
```

[查看在线TODO示例](https://jsfiddle.net/unadlib/6wabhdqp/)