# Presentation-Container-component

**Problem**
`Data and Logic together`

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {time: this.props.time};
    this._update = this._updateTime.bind(this);
  }

  render() {
    var time = this._formatTime(this.state.time);
    return (
      <h1>{ time.hours } : { time.minutes } : { time.seconds }</h1>
    );
  }

  componentDidMount() {
    this._interval = setInterval(this._update, 1000);
  }

  componentWillUnmount() {
    clearInterval(this._interval);
  }

  _formatTime(time) {
    var [ hours, minutes, seconds ] = [
      time.getHours(),
      time.getMinutes(),
      time.getSeconds()
    ].map(num => num < 10 ? '0' + num : num);

    return {hours, minutes, seconds};
  }

  _updateTime() {
    this.setState({time: new Date(this.state.time.getTime() + 1000)});
  }
}

ReactDOM.render(<Clock time={ new Date() }/>, ...);
```


**Solutions**

`Let's split the component into two parts - container and presentation.`


## Container Component


Containers know about data, it's shape and where it comes from. They know details about how the things work or the so called business logic. They receive information and format it so it is easy to use by the presentational component. Very often we use higher-order components to create containers. Their render method contains only the presentational component.
