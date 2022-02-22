# Platform module

-   docs : https://reactnative.dev/docs/platform
-   platform-specific-code : https://reactnative.dev/docs/platform-specific-code

### `Platform.OS`

-   returns string value : `android`, `ios`

```js
// e.g.
import { Platform, StyleSheet } from "react-native";

const styles = StylesSheet.create({
    height: Platform.OS === "ios" ? 200 : 100,
});
```

### `Platform.Select(config: object): any`

-   config(object) parameter keys : 'ios' | 'android' | 'native' | 'default'

```js
// e.g.
import { Platform, StyleSheet } from "react-native";

const styles = StyleSheet.create({
    container: {
        flex: 1,
        ...Platform.select({
            ios: {
                backgroundColor: "red",
            },
            android: {
                backgroundColor: "green",
            },
            default: {
                // other platform, web for example
                backgroundColor: "blue",
            },
        }),
    },
});
```
