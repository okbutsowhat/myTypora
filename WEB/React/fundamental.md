```react
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null
    }
  }
  render() {
    return (
      <button
        className="square"
        onClick={() => { this.setState({value: 'X'}) }}
      >
        {this.state.value}
      </button>
    );
  }
}
```

> 每次组件调用`setState`时, React 都会自动更新其子组件

```react
  renderSquare(i) {
    return (<Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />);
  }
```

> 注意
>
> 为了提高可读性，我们把返回的 React 元素拆分成了多行，同时在最外层加了小括号，这样 JavaScript 解析的时候就不会在 `return` 的后面自动插入一个分号从而破坏代码结构了。