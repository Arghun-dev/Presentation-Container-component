# Presentation-Container-component

**Problem**
`Data and Logic together` => **separation concerns**

`Concern`: `something your code is responsible for`

**Example:** Imagin we have weather widget component, we have 2 sections: `fetching Data` and `Displaying Data` these are 2 concerns

1. Fetching Data => `Logic`
2. Displaying Data => `UI`

**Container Component** => is for `Logic` => `WeatherWidgetContainer.js`
**Presentation Component** => is for `user interface` => `WeatherWidger.js`

What is actually happening is that `WeatherWidgerContainer.js` component is rendering the `WeatherWidget.js` component, it will render it, and will pass in any data, that the `WeatherWidget.js` needs to know about.

```js
// the container component is responsible for state, and fetching data
import React from 'react';
import WeatherWidget from './WeatherWidget';

class WeatherWidgetContainer extends React.Component {
  state = { weather: null }
  
  componentDidMount() {
    getWeather().then(weather => {
      this.setState({ weather })
    })
  }
  
  render() {
    return (
      // the presentation component is just responsible for visually displaying the widget
      <WeatherWidget weather={this.state.weather} />
    )
  }
}
```

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


**What's a Container Component Pattern?**

If presentational components are concerned with how things look, container components are more concerned with how things work. Container components are components that specify the data a presentational component should render. Instances of container components can be generated using higher order components such as connect() from Redux or createContainer() from Relay. One very important feature of components is their ability to be reused across different parts of an application. Check out the code block below:

```js
class CarList extends React.Component {
    this.state = { cars: [] };

    componentDidMount() {
      getCars(cars =>
        this.setState({ cars: cars }));
    }
    
    render() {
      return (
        <ul>
          {this.state.cars.map(e => (
            <li>{e.make}: {e.model}</li>
          ))}
        </ul>
      );
    }
  }
```

In the example above, we’ve made the `CarList` component responsible for fetchig and displaying of data but there’s a catch, `CarList` cannot be used unless under the same conditions. Let’s implement the container component pattern and solve that problem:

```js
class CarListContainer extends React.Component {
    state = { cars: [] };
    componentDidMount() {
      getCars(cars =>
        this.setState({ cars: cars }));
    }
    render() {
      return <CarsList cars={this.state.cars} />;
    }
}
```

We then recreate CarsList to take cars as a prop:

```js
const CarsList = props =>
    <ul>
      {props.cars.map(e => (
        <li>{e.make}: {e.model}</li>
      ))}
    </ul>
```

By setting apart our data fetching and rendering operations, we have made our CarsList component super reusable. Not only that, we get go understand our application’s user interface better and make our components easier to understand.

A basic recap on we have covered in this section:

`. Container components have to do with with how things work.`

`. They may contain presentational components. Presentational components don’t contain container components.`

`Provides data and behavior to presentational components and other container components.`

`Because they are mostly data sources, they are often stateful.`


**Implementing Presentational and Container Component Pattern**

Now that we have seen how presentational and container components work, let’s show how we can make our components and applications in general look more presentable using both patterns. Below is a component which is yet to be split into container and presentational:

```js
class Clock extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        time: this.props.time
      };
      this.update = this.updateTime.bind(this);
    }
    render() {
      var time = this.formatTime(this.state.time);
      return (<h1>{ time.hours } : { time.minutes } : { time.seconds }</h1>);
    }
    componentDidMount() {
      this.interval = setInterval(this.update, 1000);
    }
    componentWillUnmount() {
      clearInterval(this.interval);
    }
    formatTime(time) {
      var [hours, minutes, seconds] = [
        time.getHours(),
        time.getMinutes(),
        time.getSeconds()
      ].map(num => num < 10 ? '0' + num : num);
      return {
        hours,
        minutes,
        seconds
      };
    }
    updateTime() {
      this.setState({
        time: new Date(this.state.time.getTime() + 1000)
      });
    }
  };
  ReactDOM.render(<Clock time={ new Date() }/>, ...);
```

In the constructor of the Clock component above, the passed time object is stored to the internal state. setInterval updates the state every second and re-renders the component. However our Clock component may have a couple of problems, changing the time inside the component implies that only Clock knows the time value. Should there be another component that requires this data, it will be very hard to access it. Let’s try to rebuild the Clock component into a presentational and a container component. We first create a container component, ClockContainer:

```js
// Clock/index.js
  import Clock from './Clock.jsx'; // our presentational component

  export default class ClockContainer extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        time: props.time
      };
      this.update = this.updateTime.bind(this);
    }
    render() {
      return <Clock { ...this.extract(this.state.time) }/>;
    }
    componentDidMount() {
      this.interval = setInterval(this.update, 1000);
    }
    componentWillUnmount() {
      clearInterval(this.interval);
    }
    extract(time) {
      return {
        hours: time.getHours(),
        minutes: time.getMinutes(),
        seconds: time.getSeconds()
      };
    }
    updateTime() {
      this.setState({
        time: new Date(this.state.time.getTime() + 1000)
      });
    }
  };
```

Our `ClockContainer` component above accepts `time`, performs the setInterval loop and knows details about the data (getHours, getMinutes and getSeconds).

Our presentational component will consist of just the visual representation of our clock:

```js
// Clock/Clock.jsx
  export default function Clock(props) {
    var [hours, minutes, seconds] = [
      props.hours,
      props.minutes,
      props.seconds
    ].map(num => num < 10 ? '0' + num : num);
    return <h1>{ hours } : { minutes } : { seconds }</h1>;
  };
```

By dividing our initial Clock component in this fashion, we are able to reuse it easily in different instances. This way Clock.jsx can be present in applications that have nothing to do with keeping time or date.
