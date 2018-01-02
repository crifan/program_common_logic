# debounce防抖动

debounce的英文是**防反跳**，也叫**防止误动作**、**防抖动**

典型的用途有，键盘按键的去抖动，防抖动

类似的写前端页面时常会遇到类似的场景：

用户输入要搜索的文字而触发搜索，调用后台接口去返回数据，然后前端页面列表刷新显示

但是如果每次输入一个字符后就立刻触发后台搜索，则往往在一次性输入多个字符串时会导致中间的无效搜索，也会导致性能浪费和界面卡顿。

解决办法是：

当用户输入一个字符时，延迟触发搜索，比如延迟500ms：
* 在500ms内，如果用户没有在继续输入，再去调用接口返回搜索结果
* 在500ms内，如果用户又继续输入其他字符了，则重新触发，重新计算，再延迟500ms判断是否输入

由此可以实现：

不论用户是一次性只输入单个字符串，还是一段时间内连续输入多个字符串，最终都只是触发一次搜索，返回结果，刷新页面，使得用户体验很好。

对于上述逻辑，可以自己写代码实现：

在获得用户输入的事件时，设置一个定时器，计算倒计时。

也可以利用：

这个概念本身叫做debounce

有第三方库帮忙实现这个debounce的效果，比如lodash的debounce

比如之前写的：ReactJS中实现，延迟搜索的效果的代码：

```js
import { h, Component } from 'preact';
import style from './style.less';
import PropTypes from 'prop-types';

import _ from "lodash";

export default class Search extends Component {
  state = {
    value : null
  }

  constructor(props) {
    super(props);

    this.onInput = this.onInput.bind(this);
    this.onClick = this.onClick.bind(this);
  }

  componentWillMount() {
    this.delayedOnInput = _.debounce(this.props.onInputChange, this.props.debounceTimeout);
  }

  onInput(e){
    this.setState({value : e.target.value});

    e.persist();
    this.delayedOnInput(this.state.value);
  }

  onClick(e){
    if (this.props.onClick) {
      this.props.onClick(e);
    }
  }

  render () {
    return (
        <div class={style.cz_top_box}>
          <ul>
            <a>
              <li class={style.min_box}>
                <input
                  type="search"
                  value={this.state.value}
                  placeholder={this.props.placeholder}
                  onClick={this.onClick}
                  onInput={this.onInput} />
              </li>
            </a>
          </ul>
        </div>
    );
  }

}

Search.PropTypes = {
  debounceTimeout : PropTypes.number,
  placeholder : PropTypes.string,
  onInputChange : PropTypes.func.isRequired,
  onClick : PropTypes.func
};

Search.defaultProps = {
  debounceTimeout : 500,
  placeholder : "请输入"
};
```

