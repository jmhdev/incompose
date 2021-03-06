# compose
## Description
Use to compose multiple higher-order components into a single higher-order component.
This works exactly like the function of the same name in Redux, or lodash's `flowRight()`.

## API
```
compose(
  ...functions : Array<Function>
) : Function
```

## Example
```javascript
import {
  default as Inferno,
  linkEvent
} from 'inferno';

import {
  compose,
  withState
} from 'incompose';

const inc = (props) => {
  props.setCount(props.count += 1);
};

const dec = (props) => {
  props.setCount(props.count -= 1);
};

const Counter = (props) => (
  <div>
    <h1>count : {props.count}</h1>
    <button onClick={linkEvent(props, dec)}>-</button>
    <button onClick={linkEvent(props, inc)}>+</button>
  </div>
);

/**
 * with state creates 2 new props on the component props
 * props.count		-	contains the value (1 is set as default value)
 * props.setCount	-	contains the setter function
 */
const withCounterState = withState('count', 'setCount', 1);

/**
 * with compose all the extended functions are composed BEFORE Counter
 * gets rendered. Please not that order matters.
 */
export default compose(
  withCounterState,
)(Counter);
```