# Platform module

-   link : https://reactnative.dev/docs/platform-specific-code

### `Platform.OS`

```js
// e.g.
import { Platform, StyleSheet } from "react-native";

const styles = StylesSheet.create({
    height: Platform.OS === "ios" ? 200 : 100,
});
```

### `Platform.Select`

-   values : 'ios' | 'android' | 'native' | 'default'

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
