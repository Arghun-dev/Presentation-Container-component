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


**Let's explain more**

## Presentational and Container Component Pattern in React

**Introduction**

If you’ve been working with React for a while you’ll have to agree that building responsive applications cleanly is difficult. As more components are added, the application grows more complex and things eventually get to a point where you will have to make important decisions such as where to put your data and how to manage state. Implementing the wrong decisions will not only make your application error prone, you will also have code that’s hard to read and maintain. I’m going to discuss how container and presentational component patterns - two React patterns that are used in organizing React based applications.


**What's Presentational Component Pattern?**

Presentational Component Patterns can best be described as patterns that are primarily concerned with how things look. The primary function of a presentational component is to display data. They rarely handle state and are best written as stateless functional components. The term “presentational component” does not mean that the component is a type of class in the React library, it just implies a practice which programmers have used over time while creating component-based React user interfaces. Examples of presentational components are lists containing information and data. Check out this code block showing a school list as an example:

```js
const SchoolList = props =>
    <ol>
      {props.schools.map(s => (
        <li>{s.name} - {s.grade}</li> 
      ))}
    </ol>
```

Although best written as stateless functional components, presentational components can be made to have a degree of interactivity via the addition of callbacks. Check out this example of how an input field can be controlled to have a certain limit of characters:

```js
const milesLimit = 3
  class MilesRun extends React.Component {
    render() {
      return (
      <input
          type=number
          className="miles-run"
          onChange={this.props.onChange}
          maxLength={this.props.limit || milesLimit} 
      />
     );
    }
  }
```

In the example, `MilesRun` fits the description of a presentational component. It displays data using the <input> tag, but then it also provides a class which we can use should we want to style our input and an onChange callback as well as a limit for the amount of numbers users can enter into the field. By providing all this definition in one place, the application is made easier to work with and debug. Let’s have a recap on all we have covered so far:

`. Presentational components are primarily concerned with how things look.`

`. Most times they contain no more than a render method.`

`. Presentational components do not know how to load or alter the data that they render.`

`. Presentational components rarely have any internally changeable state properties.`

`. Presentational components best written as stateless functional components.`
