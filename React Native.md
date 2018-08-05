## React Native

### Project initialization

```
// initialize an React Native project
react-native init projectName

// run IOS simulator
react-native run-ios/run-android
```

### Styling component

__flexbox layout__

In React Native, we use `flex: 1` to specify a flexbox layout instead of `display: flex` in HTML. In addition, the defaults are different, with `flexDirection` defaulting to `column` instead of `row`.

We use flexbox properties `justifyContent` and `alignItems` to position elements within a view, however, the properites differ slightly from how we normally use for the web due to the default flex direction is `column` in React Native.

- `justifyContent` defines how an element positions in the y axis
- `alignItems` defines how an element positions in the x axis

__style object__

Usually we create a style object and then assign it to the React Native component.

```js
const Header = () => {
  const { fieldContainer, text } = styles;

  return (
    <View style={fieldContainer}>
      <Text style={text}>Albums!</Text>
    </View>
  );
};

const styles = StyleSheet.create({
    fieldContainer: {
        marginTop: 20,
        marginBottom: 20,
        backgroundColor: '#fff',
    },
    text: {
        height: 40,
        margin: 0,
        marginLeft: 7,
        marginRight: 7,
        paddingLeft: 10,
    }
});
```
__Add multiple styles to an element__

We can assign an element an array of styles `style={[styles.main, styles.override]}`, the later style will override the first style 
