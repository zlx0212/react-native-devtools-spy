[![npm version](https://badge.fury.io/js/react-native-devtools-spy.svg)](https://badge.fury.io/js/react-native-devtools-spy)
# react-native-devtools-spy

Add this package to your react native project to get an extra "Spy" development menu option.
Clicking the "Spy" button will turn on extra logs:
1. All MessageQueue messages (messages going over the javascript <-> native bridge).
2. All MobX events.

![](https://media.giphy.com/media/3o6nUVbZSX54Mo67NS/giphy.gif)

## Getting started

`$ npm install react-native-devtools-spy --save`

### Mostly automatic installation

`$ react-native link react-native-devtools-spy`

### Manual installation

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.reactlibrary.devtools.spy.SpyOptionsHandler;` to the imports at the top of the file
  - Add `import android.os.Bundle;` to the imports at the top of the file
  - Add the following lines to the `onCreate` method:
  ```
  protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (BuildConfig.DEBUG) {
            getReactInstanceManager().getDevSupportManager()
                    .addCustomDevOption("Spy",
                            new SpyOptionsHandler(
                                    getReactInstanceManager()));
        }
    } 
  ```
    
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-devtools-spy'
  	project(':react-native-devtools-spy').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-devtools-spy/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-devtools-spy')
  	```

#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-devtools-spy` and add `RNDevToolsSpy.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNDevToolsSpy.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

## Usage
Add the DevToolsSpy component to your view hierarchy:
```javascript
import DevToolsSpy from 'react-native-devtools-spy';
...
render() {
    return (
      <View>
          {__DEV__ ? <DevToolsSpy /> : null}
      </View>
    );
  }
```
Run your application and open the development menu (shake/cmd+d/cmd+m).
Click on the "Spy" menu item.
Toggle the "Message queue" button.
In the console logs you will see the extra logs of all messages going over the native <-> javascript bridge, as such:
```
N->JS : RCTEventEmitter.receiveTouches(["topTouchStart",[{"target":9,"locationX":62.5,"pageY":389,"force":0,"locationY":22,"pageX":197,"identifier":1,"timestamp":190140621.128411}],[0]])
JS->N : UIManager.measure([12,33391])
JS->N : NativeAnimatedModule.createAnimatedNode([3,{"type":"value","value":1,"offset":0}])
JS->N : NativeAnimatedModule.createAnimatedNode([2,{"type":"style","style":{"opacity":3}}])
```
If you are using MobX inyour project also toggle the "MobX" button to log all MobX events, as such:
```
[MobX] {type: "update", object: {…}, oldValue: 1517169620887, name: "text1", newValue: 1517169740502, …}
[MobX] {object: Reaction, type: "scheduled-reaction"}
[MobX] {spyReportEnd: true}
[MobX] {object: Reaction, type: "reaction", fn: ƒ, spyReportStart: true}
[MobX] {time: 3, spyReportEnd: true}
```
