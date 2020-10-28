# React Native

```javascript
```

- Install
- EXPO
- Props
- Style
- React Hooks
- Components
- Navigation
- Form
- Methods/Objects
- Fonts
- Images
- Icons
- Firebase

## Install

---

- create a new project :
  - expo init
  - **or**
  - react-native create (not sure)
- cd [app-folder]
- npx react-native run-android **or** (with expo) npm start

- wait for the Tunnel to be ready then scan the QR code

---

## EXPO

---

### ScreenOrientation

- Locking the display in a given orientation:
  `ScreenOrientation.lockAsync(ScreenOrientation.OrientationLock.[PORTRAIT || LANDSCAPE])`

### Platform

- Checking what platform the app is on :
  `Platform.OS === '[android || ios]'` then run the desired code
- Using _select_ :
  `Platform.Select({ios: styles.[propertyNameIOS], android: styles.[propertyNameAndroid]})`

### linear-gradient

`npm install --save expo-linear-gradient`
`import {linear-gradient} from 'expo-linear-gradient`
`<LinearGradient color={['[color1],[color2]']} ></LinearGradient>`

---

## Props

---

- Data that are passed to the children components
- props.style = nested component
- props.children = nested component
  i.e:

```javascript
  <Text style={styles.[styleVariableName]} >{props.children}</Text>
```

---

## Style

---

- import {StyleSheet} from 'react-native'
  `const styles = StyleSheet.create({})`
- Create a _constants_ folder with Colors, Spacing, Sizes, etc files and set variables in them

Box-shadow:
**iOS** : shadowColor: string, shadowOpacity: number, shadowOffset: { width: number, height: number }, shadowRadius: number,
**Android** : elevation

_responsive module_ with dimensions API

---

## React Hooks

---

### useState()

- defines a variable and sets a value
  i.e: const [propValue, setPropValue] = useState( **initial propValue** )

### useEffect()

useEffect(() => {}): will run **after** the component has been rendered and **after** every render cycle.  
Keeps the value of a variable even when the component is destroyed

### useCallBack()

Prevents infinite loop

---

## Components

---

### Template

`import React from "react";`
`import {StyleSheet} from "react-native";`
`const Template = props => { return()}`
`const styles = StyleSheet.create({})`
`export default Template;`

### View :

- can be used everywhere to wrap some other components. Uses flexbox with columns as default flex direction.

### Text :

- required to display some text, can be nested and will inherit the styles from its parent component. Doesn't use flexBox like _view_ does
- numberOfLines={_number_} to break the text if too long

### Button

- color={}
- onPress={props.function}
- title={}

### TouchableOpacity:

- onPress={} : onPress method with opacity visual feedback
- activeOpacity={_value_}

### TouchableWithoutFeedback

- onPress={} : Allows an onPress method without any graphical display

### TouchableNativeFeedback : Android only

- dislays the device native style :
  => For exemple On Android, a button will have a ripple effect but not on iOS.

```javascript
import { TouchableNativeFeedback, Platform } from "react-native";
```

### Modal:

- visible={} : true/false Toggles the visibility

### TextInput :

#### Properties

- onChangeText={() => {}}
- onKeyChange={}: Triggers an event on each key press
- Keyboard.dismiss() to hide the keyboard
- keyboardType='[value]' : manages the type of keyboard
- autoCapitalize="none"
- autoCorrect
- returnKeyType='next'
- onEndEditing={() => console.log('onEndEditing')}
- onSubmitEditing={() => console.log('onSubmitEditing')}

#### Validations

Add a validation function to the onChangeText property:

```javascript
// input property
onChangeText={[inputChangeHandler]}

// new state
const [inputIsValid, setInputIsValid] = useState([defaultValue])

// input handler
const [inputChangeHandler] = inputValue => {
 setInputIsValid(inputValue.trim().length)
}

// Add error message below the input
{!inputIsValid && <Text>Error message</Text>}
```

### Image :

```javascript
<Image source={{ uri: [imageSource] }} />
```

### ImageBackground :

- sets a background image. Needs to wrap the element with the background:
  ```javascript
  <ImageBackground source={{ uri: [imageUrl] }}>
    <Element></Element>
  </ImageBackground>
  ```

### ScrollView :

- makes a view scrollable. Use if the list length is predefined
- Takes an array and maps over it: {someList.map(item => {<ComponentToDisplay key={uniqueValue}>})}
- Make sure that if the ScrollView is wrapped around a view, the view has a flex: 1 style
- contentContainerStyle={} to style the ScrollView
- Use **flexGrow** rather than **flex**

### FlatList:

- makes a view scrollable but more effective than _Scrollable_ as it will not rerender the whole list on each update
  Params:
- keyExtractor: the id of each item
- data: array from which the list will be displayed
- renderItem: each item of the array that will be rendered. Usualy a function, **must not be called - no _()_**
- numColumns: number of columns, default is one
  ```javascript
  <FlatList
    keyExtractor={item => item.id}
    data={[arraySource]}
    renderItem={[itemToRender]}
    numColumns={[number]}
  />
  ```

### KeyboardAvoidingView

Moves the input chosen if the soft keyboard overlaps it
Wraps the form:

```javascript
<KeyboardAvoidingView
  style={{ flex: 1 }}
  behavior="[property]"
  keyboardVerticalOffset={[number]}
></KeyboardAvoidingView>
```

### ActivityIndicator

Will show a native loading spinner

```javascript
<ActivityIndicator
  size="large || medium || small"
  color={[color]}
></ActivityIndicator>
```

---

## Navigation

---

- Stack navigation
- Tab navigation
- Drawer navigation
- Switch navigation

npm install --save

- react-navigation

expo install

- react-native-gesture-handler
- react-native-reanimated

`npm install --save react-navigation-stack`  
`npm install --save react-navigation-tabs`  
`npm install --save react-navigation-drawer`
`npm install --save react-navigation-switch`

Enabling navigation to nested components:

```javascript
<NestedComponent navigation={props.navigation} />
```

### Stack Navigator

All routes created are _stacked_. The current view is the one on top and the previous is the one below. Simple back and forth navigation. A **stack** navigator will add a header to the screen.

```javascript
import { createStackNavigator } from "react-navigation-stack";
```

- Creating the routes:  
  `createStackNavigator({customKeyName: ComponentName})`
- Exporting the default component that will be loaded:  
  `createAppContainer(ComponentName)`
- Navigating to a new screen:  
  `props.navigation.navigate({routeName: [routeName]})`  
  **or**  
  `props.navigation.navigate([routeName])`
- Navigating back:  
   `props.navigation.pop()` _(only available on stack navigation)_  
   **or**  
  `props.navigation.goBack()`
- Navigating to route screen:  
  `props.navigation.popToTop()`
- Setting options in the component file:
  ```javascript
  ComponentName.navigationOptions = {
    headerTitle: [string],
    headerStyle: {
      backgroundColor: [value]
      }
    headerHintColor: [value],
    headerRight: <ComponentName>
  }
  ```
- Setting default navigation options in the navigator file:  
  `defaultNavigationOptions: {`
  `headerStyle: {`
  `backgroundColor: [value]`
  `}`
  `headerHintColor: [value],`
  `}`

#### Passing Data to nested component

Forward data from the parent component with the navigate method:

```javascript
props.navigation.navigate([routeName], {
  [dataName]: [valueToTransfer]
});
```

Getting the data in the nested component:

```javascript
const [variabelName] = props.navigation.getParam("[dataName]");
```

### Tabs Navigator

```javascript
import { createBottomTabNavigator } from "react-navigation-tabs";
```

```javascript
const CustomTabNavigator = createBottomTabNavigator(
  [CustomLabel]: {
    screen: FavoritesScreen,
    navigationOptions: {
      tabBarIcon: tabInfo => {
        return (
          <Ionicons name="[iconName]" size={number} color={tabInfo.tintColor} />
        );
      },
      tabBarLabel: "[overrideCustomLabel]"
    }
  },
  {
    tabBarOptions: {
      activeTintColor: [customColor]
    }
  }
)
```

=> Android style:
`npm install --save react-navigation-material-bottom-tabs`
`npm install --save react-native-paper`

```javascript
import { createMaterialBottomTabNavigator } from "react-navigation-material-bottom-tabs";
```

#### Header buttons

`npm install --save react-navigation-header-buttons`
`import { HeaderButtons, Item } from "react-navigation-header-buttons";`

```javascript
ComponentName.navigationOptions = {
  headerRight: (
    <HeaderButtons HeaderButtonComponent={HeaderButton}>
      <Item title="[customTitle]" iconName="[iconName]" onPress={() => {}} />
    </HeaderButtons>
  )
};
```

### Drawer Navigator

```javascript
import { createMaterialBottomTabNavigator } from "react-navigation-material-bottom-tabs";
```

## useScreen _depracated_

## enableScreens instead

Improves transitions between screens.  
Call it on the root file before any JSX code is ran.  
`npm install --save react-native-screens`
`import {useScreen} from 'react-native-screens'`
`useScreens()`

### Switch navigator

Will accept only one screen at a time and will not allow to go back to the main screen. Perfect for authentication

---

## Form

---

TextInput
KeyboardAvoidingView

### TextInput :

#### Properties

- onChangeText={() => {}}
- onKeyChange={}: Triggers an event on each key press
- Keyboard.dismiss() to hide the keyboard
- keyboardType='[value]' : manages the type of keyboard
- autoCapitalize="none"
- autoCorrect
- returnKeyType='next'
- onEndEditing={() => console.log('onEndEditing')}
- onSubmitEditing={() => console.log('onSubmitEditing')}

#### Validations

Add a validation function to the onChangeText property:

```javascript
// input property
onChangeText={[inputChangeHandler]}

// new state
const [inputIsValid, setInputIsValid] = useState([defaultValue])

// input handler
const [inputChangeHandler] = inputValue => {
 setInputIsValid(inputValue.trim().length)
}

// Add error message below the input
{!inputIsValid && <Text>Error message</Text>}
```

---

## Methods/Object

---

### Alert

Creates an **Alert** pop up :
`Alert.alert("Title Text", 'Description', [`
`{ text: 'Button title', style: 'button style' }`
`]);`

### Dimensions

Gets the size of the **window/screen** of the device.
It can be used anywhere in the .js file:
`Dimensions.get('window)`
To listen to orientation change event: `Dimensions.addEventListener('change', functionName)`
Then use **removeEventListener** to reset the function
To change the style everytime the device changes orientation, use **useState()** and **useEffect()**
Check lessons **90-100**

### SafeAreaView

Needs to wrap the parent component of the view. It can be the entire app(**App.js**) but it can be managed by another library when using _navigation_

**In case of errors :**  
npm i react-native-safe-area-view react-native-safe-area-context &&
react-native link react-native-safe-area-context

npm install --save @react-native-community/masked-view

### toFixed([number])

Specifies the number of decimal places

### ScreenOrientation

See the _EXPO_ section

### Pull to refresh

```javascript
onRefresh={[functionToTrigger]}
refreshing={[booleanState]}
```

---

## Fonts

---

**EXPO**

- expo install expo-font
- Add a **Fonts** folder in assets.
- Then paste font files
- In the **App.js** component :
  - import \* as Font from 'expo-font' use **Font**
  - import { AppLoading } from "expo"

**AooLoading** will wait until the required component/data/asset is loaded before displaying the app. Very useful in case of asynchronous fetching.

- Create a function :

```javascript
const fetchFonts = () => {
  return Font.loadAsync({
    "open-sans": required("./assets/fonts/OpenSans-Regular.ttf"),
    "open-sans-bold": required("./assets/fonts/OpenSans-Bold.ttf")
  });
};
```

- Then :
  `const [dataLoaded, setDataLoaded] = useState(false); if (!dataLoaded){ return ( <AppLoading startAsync={fetchFonts} onFinish={() => { setDataLoaded(true); console.log("finish"); }} onError={err => console.log("error :", err)} /> ); }`

---

## Image

---

import {Image} from 'react-native'
<Image source={} />

### Local

`<Image source={ require(_relative-path-to-image_) } />`

### Web

`<Image source={uri: 'path-to-image'} />`

---

## Icons

---

```javascript
import {[libraryName] } from '@expo/vector-icons';
<libraryName name='icon-name' size={number} color={string/HEX} />
```

---

## Firebase

---

### Redux Thunk

`npm install --save redux-thunk`
Redux Middleware: changes the actions creaters, enables asynchronous behavior like:

- sending an http request and then dispatch the action

Import **applyMiddleware** from 'redux'

```javascript
const store = createStore(rootReducer, applyMiddleware(ReduxThunk));
```

### Get

```javascript
fetch("[url]", { method: "Get" });
```

### Post

```javascript
fetch("[url]", {
  method: "Post",
  headers: {
    "Content-type": "application/json",
    body: JSON.stringify({
      keyValue: "[value]"
    })
  }
});
```

### Delete

```javascript
fetch("[url]", { method: "Delete" });
```

### Update

```javascript
fetch("[url]", {
  method: "Patch",
  headers: {
    "Content-type": "application/json",
    body: JSON.stringify({
      keyValue: "[value]"
    })
  }
});
```
