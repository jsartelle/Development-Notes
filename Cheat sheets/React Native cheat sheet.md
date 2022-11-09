[Introduction · React Native](https://reactnative.dev/docs/getting-started)

# React Fundamentals

- still need to `import React from 'react'` in each file
- `index.js` is the starting point for React Native apps, always required (even if it just `import`s other files)

# Native Components

- `<View>` is non-scrolling, `<ScrollView>` is scrolling
- `<Text>` is for non-editable text (equivalent to `<p>`), `<TextInput>` is an editable text field

## Handling Text Input

- `<TextInput>` has *onChangeText* and *onSubmitEditing* props

## Using a ScrollView

- set *horizontal* property to allow horizontal scrolling
- *pagingEnabled* lets you use paging
    - `<ViewPager>` component can be used to swipe horizontally between views
- on iOS a `<ScrollView>` with a single item can allow the user to zoom in and out if the *minimumZoomScale* and *maximumZoomScale* props are set
- all elements are rendered even if off screen - if there are a large number of items use `<FlatList>` instead (see [[#Using List Views]])

## Using List Views

- `<FlatList>` displays a scrolling list of changing, but structurally similar data
    - *data* prop is an array of data, *renderItem* prop is a function that takes a single data item and returns the rendered component
    - number of items can change over time
    - only renders elements that are currently visible
- `<SectionList>` lets you render a list broken into sections with section headers
    - takes a *sections* prop which is an array of objects (one per section), with a *title* property (the section title) and *data* property (array of data)
    - *renderItem* prop as described above

# Design

## StyleSheet

- lets you store styles similar to CSS
- static `compose(style1, style2)` method lets you combine two style objects so that the second overrides the first
- static `flatten(styleObjects)` is similar, but takes an array, and returns an object rather than a *StyleSheet*
- built-in helpers:
    - `StyleSheet.absoluteFill` - position absolute, top/bottom/left/right 0
        - also `absoluteFillObject`, no difference currently
    - `StyleSheet.hairlineWidth` - the width of a thin line on the current platform

```jsx
import React from "react";
import { StyleSheet, Text, View } from "react-native";

const App = () => (
  <View style={styles.container}>
    <Text style={styles.title}>React Native</Text>
  </View>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 24,
    backgroundColor: "#eaeaea"
  },
  title: {
    marginTop: 16,
    paddingVertical: 8,
    borderWidth: 4,
    borderColor: "#20232a",
    borderRadius: 6,
    backgroundColor: "#61dafb",
    color: "#20232a",
    textAlign: "center",
    fontSize: 30,
    fontWeight: "bold"
  }
});

export default App;
```

## Layout

- style properties are written with camelCase
- all dimensions are unitless, density-independent pixels (or percent)
- `position` can be *relative* (default) or *absolute*
- flexbox works with a few differences:
    - `flex` only accepts a single number (`flex: 2` in React Native is equivalent to `flex: 2 1 0%` on the web)
    - `flexDirection` defaults to *column*
    - `alignContent` defaults to *flex-start*
    - `flexShrink` defaults to 0

## Images

- To use an image (or other asset), require it with a relative path
    - image path must be known statically

```jsx
// GOOD
<Image source={require('./my-icon.png')} />;

// BAD
var icon = this.props.active
  ? 'my-icon-active'
  : 'my-icon-inactive';
<Image source={require('./' + icon + '.png')} />;

// GOOD
var icon = this.props.active
  ? require('./my-icon-active.png')
  : require('./my-icon-inactive.png');
<Image source={icon} />;
```

- *source* can also take an object with a `uri` property - this can be a URL, asset catalog name, or `data:` URL
- Use `@2x` and `@3x` suffixes for different screen densities (ex. `my-icon@2x.png`)
- images imported with require() are automatically sized, others need to be sized manually
    - to scale a require() image dynamically, may need to manually style it with `{ width: undefined, height: undefined }`
- `<ImageBackground>` component takes the same props as `<Image>` but can have children

## Colors

- use the `PlatformColor` function to get platform-specific named colors
    - first value is the default, rest are fallbacks

```jsx
const styles = StyleSheet.create({
  label: {
    padding: 16,
    ...Platform.select({
      ios: {
        color: PlatformColor('label'),
        backgroundColor:
          PlatformColor('systemTealColor'),
      },
      android: {
        color: PlatformColor('?android:attr/textColor'),
        backgroundColor:
          PlatformColor('@android:color/holo_blue_bright'),
      },
      default: { color: 'black' }
    })
  },
```

# Networking

- Fetch is available, or libraries based on XMLHttpRequest can be used
    - no CORS
- WebSockets are supported

# Touch

- `<Button>` is a styled button with an *onPress* prop
- For custom buttons, wrap a single element with a `Touchable` wrapper:
    - `<TouchableHighlight>` darkens when you tap (suitable for most buttons)
    - `<TouchableOpacity>` reduces opacity when you tap
    - `<TouchableNativeFeedback>` (Android only) does the Material Design ripple
    - `<TouchableWithoutFeedback>` provides no feedback
- use *onLongPress* on any `Touchable` for long presses
- `<Pressable>` is more flexible
    - supports an optional *hitSlop* property ([Rect](https://reactnative.dev/docs/rect) or number) to allow presses outside the child element
    - ![Diagram of the onPress events in sequence.](https://d33wubrfki0l68.cloudfront.net/436d715612d6a5ab228b9fd41f33f799f0c3e6d3/40bdd/docs/assets/d_pressable_pressing.svg)

# Navigation

## react-navigation

- install `@react-navigation/native`, `@react-navigation/native-stack`, `react-native-screens`, `react-native-safe-area-context`
- wrap whole app in `<NavigationContainer>`

```jsx
// In App.js in a new project

import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} options={{ title: 'Overview' }} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

- `<Stack.Screen>` is a screen of your app, takes a *name* prop and a *component* prop for the page component
    - route name casing doesn't matter
    - *component* receives a prop called *navigation* with navigation methods, ex. `navigation.navigate` to go to another screen
    - to pass additional props to a screen, wrap the Navigator with a [[React cheat sheet#Context |context provider]] and access it inside the screen components

# Platform Specific Code

- `Platform.OS` can be used to test for platform (*ios* or *android*)

```jsx
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  height: Platform.OS === 'ios' ? 200 : 100
});
```

- `Platform.select` method returns the most fitting key for the platform (`ios' | 'android' | 'native' | 'default'`)

```jsx
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red'
      },
      android: {
        backgroundColor: 'green'
      },
      default: {
        // other platforms, web for example
        backgroundColor: 'blue'
      }
    })
  }
});

/* can also be used to return platform-specific components */

const Component = Platform.select({
  ios: () => require('ComponentIOS'),
  android: () => require('ComponentAndroid')
})();

<Component />;
```

- `Platform.Version` returns the Android API version number (ex. `25`) or iOS version string (ex. `'10.3'`)
- Create platform-specific imports by ending them in `.ios.js` or `.android.js`
    - Use the `.native.js` extension for files that are the same on Android and iOS, but not used on web, and use `.js` for the web version

```jsx
// imports BigButton.ios.js or BigButton.android.js depending on platform
import BigButton from './BigButton';
```

# Workflow

- if using the development server, shake your device to open the Developer menu (or use Cmd-D for iOS simulator and Cmd-M fofr Android)
- Fast Refresh tries to reload only edited components and preserve state
    - to force remounting on every edit (for example, testing an intro animation) add `// @refresh reset` anywhere in the file
- to debug remotely, select *Debug JS Remotely* from the developer menu
    - React DevTools extension doesn't work, but the [standalone version](https://github.com/facebook/react/tree/main/packages/react-devtools) does
    - the component selected in the React devtools is available in the Chrome console as `$r` (make sure the Chrome console dropdown says `debuggerWorker.js`)
    - for the iOS version use the native Safari debugging under Develop -> Simulator -> JSContext
        - choose *Automatically Show Web Inspectors for JSContexts*
- display console logs by running `npx react-native log-ios` (or `log-android`)
    - Expo projects will automatically show logs in the terminal

# See also

- [[React cheat sheet]]
