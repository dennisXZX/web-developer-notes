## React Native

#### Project initialization

```
// initialize an React Native project
react-native init projectName

// run IOS simulator
react-native run-ios
```

#### Styling component

Usually we create a style object and then assign it to the React Native component.

__Note__: We use flexbox properties `justifyContent` and `alignItems` to position elements within a view, however, the properites differ slightly from how we normally use for the web.

- `justifyContent` defines how an element positions in the y axis
- `alignItems` defines how an element positions in the x axis

```js
const Header = () => {
  const { textStyle, viewStyle } = styles;

  return (
    <View style={viewStyle}>
      <Text style={textStyle}>Albums!</Text>
    </View>
  );
};

const styles = {
  viewStyle: {
    height: 160,
    backgroundColor: '#f8f8f8',
    justifyContent: 'center',
    alignItems: 'center'
  },
  textStyle: {
    fontSize: 20
  }
};
```
