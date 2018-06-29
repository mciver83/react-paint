<img src="https://devmounta.in/img/logowhiteblue.png" width="250" align="right">

# Project Summary

In this project, we will create a paint app that will allow us to practice passing data from one component to a child component via props, and update a parent components state from an event on a child component.

# Live Example

## Setup

1.  `create-react-app react-paint`.
2.  `cd` into the project directory.
3.  Run `npm install`.
4.  Remove service worker from `src/index.js`
5.  Run `npm start` (you might need to run `npm install` as well).
6.  In a seperate terminal, `cd` into the project directory.

## Step 1

### Summary

In this step, we will create the basic structure for the components we will be using.

### Instructions

* Create a components folder inside of src.
* Inside the components folder, create the following component files:
    * PaintCanvas.js
    * ColorPicker.js
    * Square.js
* Create a class component for Square that renders a `div` with these styles `{ height: 10, width: 10, border: '1px solid black }`
* Create a stateless functional component from ColorPicker that, for now, will render a div with 4 buttons that say 'blue', 'yellow', 'green', 'purple'.
* Create a class component for PaintCanvas that will render both the ColorPicker component and the Square component.
* In `src/App.js`, render the PaintCanvas component.

### Solution

<details>

<summary> <code> ./src/App.js </code> </summary>

```js
import React, { Component } from 'react';

import PaintCanvas from './components/PaintCanvas'

class App extends Component {
  render() {
    return (
      <div>
        <PaintCanvas />
      </div>
    );
  }
}

export default App;
}
```

</details>

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {

  render() {
    return (
      <div>
        <ColorPicker />
        <Square />
      </div>
    )
  }
}
```

</details>

<details>

<summary> <code> ./src/components/ColorPicker.js </code> </summary>

```js
import React from 'react'

export default function ColorPicker(props) {
  return (
    <div>
      <button>blue</button>
      <button>yellow</button>
      <button>green</button>
      <button>purple</button>
    </div>
  )
}
```

</details>

<details>

<summary> <code> ./src/components/Square.js </code> </summary>

```js
import React, { Component } from 'react';

export default class Square extends Component {

  render() {
    return (
      <div style={{
        height: 10, 
        width: 10, 
        border: '1px solid black'
      }}></div>
    )
  }
}
```

</details>

## Step 2

### Summary

In this step, we will be using state to keep track of the current selected color.

### Instructions

* Add `constructor` method to PaintCanvas.
    * method should call super()
    * create initial state with the following properties:
          - selectedColor: 'blue'
* Create a method to update the selected color on the component's state.  Name the method `changeSelectedColor`.  The method will take in a color as a parameter and use `this.setState` to update the `selectedColor` property on state.  Rememer to bind `this`!

### Solution

<details>

<summary> <code> ./src/components/PaintCnavas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  render() {
    return (
      <div>
        <ColorPicker />
        <Square />
      </div>
    )
  }
}
```

</details>

## Step 3

### Summary

In this step, we will make it so when we click on a color in `ColorPicker`, it will updated the `selectedColor` property on `PaintCanvas` state.

### Instructions

* In PaintCanvas.js, pass the `changeSelectedColor` method as a prop on `ColorPicker`.  Name the prop `handleColorClick`.
* In ColorPicker.js, add an `onClick` event listener to each of the buttons, that takes a callback function that will invoke the handleColorClick method from the props object, and pass in the color of that button. 

### Solution

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  render() {
    return (
      <div>
        <ColorPicker handleColorClick={this.changeSelectedColor}/>
        <Square />
      </div>
    )
  }
}
```

</details>

<details>

<summary> <code> ./src/components/ColorPicker.js </code> </summary>

```js
import React from 'react'

export default function ColorPicker(props) {
  return (
    <div>
      Color Picker
      <button onClick={() => props.handleColorClick('blue')}>blue</button>
      <button onClick={() => props.handleColorClick('yellow')}>yellow</button>
      <button onClick={() => props.handleColorClick('green')}>green</button>
      <button onClick={() => props.handleColorClick('purple')}>purple</button>
    </div>
  )
}
```

</details>

## Step 4

### Summary

In this step, we'll make use of `axios` to get the `SOLD!` button to work. When deleting data on a server you should always use a DELETE request.

### Instructions

* Open `./src/App.js`.
* Locate the pre-made `sellCar` method.
* Using `axios` and the API documentation make a DELETE request to delete ( "sell" ) a vehicle.
  * If the request is successful, use `this.setState()` to update the value of `vehiclesToDisplay` and use `toast.success`.
    * Hint: Inspect the returned data object.
  * If the request is unsuccessful, use `toast.error`.

### Solution

<details>

<summary> <code> ./src/App.js ( sellCar method ) </code> </summary>

```js
sellCar( id ) {
  axios.delete(`https://joes-autos.herokuapp.com/api/vehicles/${ id }`).then( results => {
    toast.success("Successfully sold car.");
    this.setState({ 'vehiclesToDisplay': results.data.vehicles });
  }).catch( () => toast.error("Failed at selling car.") );
}
```

</details>

## Black Diamond

If there is extra time during the lecture, try to complete the remaining methods. The remaining methods can also be used as `axios` and `CRUD` practice on your own time.

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2017. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://devmounta.in/img/logowhiteblue.png" width="250">
</p>
