# Multi Input Input

[![Greenkeeper badge](https://badges.greenkeeper.io/penx/multi-input-input.svg)](https://greenkeeper.io/)

A React higher order component that modifies a given component containing multiple inputs to behave as though it was a single input by:
- merging values in to an object
- not calling onBlur when going from field to field within the same component
- not calling onFocus when going from field to field within the same component

# Why?

See
https://medium.com/@penx/form-elements-in-presentational-component-packages-a618e9aa7416

## Example problem

You have 3 inputs that combined together form a date. You have these 3 input inside a single styled component and want to plug this in to your form code, e.g. using Redux Form or Final Form.

You want the 3 inputs to behave as though they were a single input: having a single value prop and handling the onBlur, onFocus, onChange events as though they were a single input, as well as allowing the parent to [get refs](https://reactjs.org/docs/refs-and-the-dom.html) to each individual input.


# Usage:

For example, 3 inputs could be combined to form a single date input:

```jsx
import React from 'react';
import PropTypes from 'prop-types';

import multiInputInput from 'multi-input-input';

class OptionalDateInput extends React.Component {
  inputs = {}

  renderInput(label, key) {
    return (
      <label>
        {label}:
        <input
          value={(this.props.value && this.props.value[key]) || ''}
          onChange={e => this.props.onChange(e, key)}
          onBlur={e => this.props.onBlur(e, key)}
          onFocus={e => this.props.onFocus(e, key)}
          ref={(input) => { this.inputs[key] = input; this.props.refs(this.inputs); }}
        />
      </label>);
  }

  render() {
    return (
      <div>
        {this.renderInput('Day', 'day')}
        {this.renderInput('Month', 'month')}
        {this.renderInput('Year', 'year')}
      </div>
    );
  }
}

OptionalDateInput.propTypes = {
  value: PropTypes.shape({
    day: PropTypes.number,
    month: PropTypes.number,
    year: PropTypes.number,
  }),
  refs: PropTypes.func,
  onChange: PropTypes.func,
  onBlur: PropTypes.func,
  onFocus: PropTypes.func,
};

OptionalDateInput.defaultProps = {
  value: {
    day: null,
    month: null,
    year: null,
  },
  refs: () => null,
  onChange: () => null,
  onBlur: () => null,
  onFocus: () => null,
};

export default multiInputInput(OptionalDateInput);
```


See https://github.com/penx/pcp-form-example/blob/master/packages/presentational-components/src/components/input/OptionalDateInput.js
