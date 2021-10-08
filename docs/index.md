# Zabaan for React Native

Development of Zabaan in react native is still in initial stages.
We have added basic functionality of Zabaan Native in a much simpler fashion.

To add the node module for Zabaan you'll have to run below command in your terminal.

```bash
npm install zabaan-react-native
```

All the details related to react native package and latest version updates can be found on npmjs website [https://www.npmjs.com/package/zabaan-react-native](https://www.npmjs.com/package/zabaan-react-native)

In screen where you want to display Zabaan. you have to import following Dependency in your JS files. This will enable you to use functions from Zabaan to configure Zabaan assistant.

```jsx
import Zabaan from 'zabaan-react-native';
```

## Initialization

Zabaan needs to be initialized across the app before app can use its feature. You will have to import Zabaan in very first screen of your app. Below function needs to be called in order to initialize Zabaan assistant.

```java
async function onInit() {
        if (!init) {
            Zabaan.setDebug(true);
            Zabaan.setStatusBarHeight(StatusBar.currentHeight);
            Zabaan.init('Access_Token_From_Zabaan_CMS',
                function (complete) {
                    setInit(true);
                    Zabaan.setDefaultFingerAnimation(3)
                }
            );
        }
    }
```

If Zabaan gets initialized properly, init function will return a callback. Once this callback is received it ensures that Zabaan is initialized and can we used anywhere in the application. To understand what each Zabaan function does, Please check [this](#list-of-methods-exposed-by-zabaan) section.

## Enabling Zabaan Assistant for Screen

After importing Zabaan module in particular file screen file where assistant is to be displayed. function `Zabaan.show()` must be called in order to show assistant for that screen.

Zabaan SDK uses the concept of screen and state to identify and set interactions needed to be played for a particular screen and state.

To identify a particular screen `setScreenName(screen_name)` must be used. Each interaction defined over the CMS should have a screen name and state defined.

Below is a wrapper functions that combines all the function that enables Zabaan assistant to identify on which screen it is loaded and play correct interactions whenever user interacts with assistant.

```java
async function enableForScreen() {
        Zabaan.show();
        Zabaan.setScreenName('Screen_Name');
        Zabaan.setState('State');
}
```

You have to set appropriate values to Zabaan functions inside this wrapper and call this function when the screen loads. You can use `onEffect()` method provided by react and call this wrapper function as follows. To understand more about how the each method used in the wrapper function, Please check [this](#list-of-methods-exposed-by-zabaan) section.

```jsx
useEffect(() => {
        const unsubscribe = navigation.addListener('focus', () => {
            //enabling Zabaan assistant by calling wrapper function
            enableForScreen();
        });
        return unsubscribe;
   }, [navigation]);
```

## Setting Zabaan language Code

By default, the Zabaan SDK comes packaged with a language selection screen. If the `Zabaan` language code is not set and the user clicks on the assistant, it will automatically show the screen shown below and prompt the user to select the language of their choice.

## Un-linking Zabaan Language Code from App Language Code

If the Zabaan `languageCode` has not been set, clicking on the assistant will show the user a popup language selection screen. This will serve as a way for users to select the language in which they would like the assistant to speak to them. If you would like to prevent the language selection screen from popping up.

In the event that you would like to programmatically set the `languageCode` (thus preventing the language screen popup) you can use the following:

```java
//Language code is a two digit ISO certified code for that language.
Zabaan.setLanguage("en");

```

List of language codes currently supported by Zabaan are mentioned as follows

```kotlin
"hi" -> HINDI
"en" -> ENGLISH
"kn" -> KANNADA
"te" -> TELUGU
"bn" -> BENGALI
"gu" -> GUJARATI
"pa" -> PUNJABI
"mr" -> MARATHI
"ta" -> TAMIL
"ml" -> MALAYALAM
"or" -> ODIA
"as" -> ASSAMESE
"hi-en" -> HINGLISH
```

## List of methods exposed by Zabaan

1. `Zabaan.setDebug(Boolean)` - For debugging purposes, pass `true` to see logs. (Recommended to set `false` while shipping the app)
2. `Zabaan.setStatusBarHeight(Integer)` - For apps using full screen they can pass `0` and other applications can pass the height of StatusBar here (Recommended to get height directly from `react-native.StatusBar.StatusBar.currentHeight`)
3. `Zabaan.init(String, Callback(Boolean))` - To initialize `Zabaan` pass your access token here, the callback will return `Boolean` value. **Important** thing to remember is to make sure this method is called only once during the entirety of the lifecycle.
4. `Zabaan.setDefaultFingerAnimation(Integer animvalue)` - This method accepts Integer values between 1 to 6, representing 6 different types of animation. The value passed to this animation will become the default animation through out the app.
5. `Zabaan.show()` - This method is responsible for showing `Zabaan` assistant on your Screen. **Note** - First call to this method can take a little time as we make a few Async calls internally and assistant will be visible only once those calls have been completed.
6. `Zabaan.hide()` - This method will hide the `Zabaan`assistant from the screen.
7. `Zabaan.setScreenName(String)` - This method accepts string and is responsible for setting `ScreenName` for `Zabaan` assistant.
8. `Zabaan.setState(String)` - This method accepts string and is responsible for setting `State` for `Zabaan` assistant.
9. `Zabaan.setLanguage(String)` - Programmatically set language code to be used by `Zabaan` assistant.
