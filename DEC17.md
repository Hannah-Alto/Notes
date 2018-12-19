### Type checking with prop-types in Jest
[Reference: https://medium.com/shark-bytes/type-checking-with-prop-types-in-jest-e0cd0dc92d5](https://medium.com/shark-bytes/type-checking-with-prop-types-in-jest-e0cd0dc92d5)

`Warning: Failed prop type: CustomText: prop type 'styleOverride' is invalid; it must be a function, usually from the 'prop-types' package, but received 'object'.`


Or use yarn test --coverage --silent


### check value
`    expect(wrapper.find('Text').prop('children')).toBe('CONTENT')`

### Call a prop function, eg. onPress()
`     wrapper
      .find(Text)
      .props()
      .onPress()`
and then do something similar to:
`    expect(spy1).toHaveBeenCalledWith(link)`


