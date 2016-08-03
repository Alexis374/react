在`componentDidMount`中获取数据，当响应到来时，在state中保存数据，触发渲染方法来更新界面。
当异步获取数据时，用`componentWillUnmount`在组件(从DOM上)卸载前取消所有未解决（未收到响应的）请求。
下面的代码获取特定github用户的最新gist：
```js
var UserGist = React.createClass({
  getInitialState: function() {
    return {
      username: '',
      lastGistUrl: ''
    };
  },

  componentDidMount: function() {
    this.serverRequest = $.get(this.props.source, function (result) {
      var lastGist = result[0];
      this.setState({
        username: lastGist.owner.login,
        lastGistUrl: lastGist.html_url
      });
    }.bind(this));
  },

  componentWillUnmount: function() {
    this.serverRequest.abort();
  },

  render: function() {
    return (
      <div>
        {this.state.username}'s last gist is
        <a href={this.state.lastGistUrl}>here</a>.
      </div>
    );
  }
});

ReactDOM.render(
  <UserGist source="https://api.github.com/users/octocat/gists" />,
  mountNode
);
```
