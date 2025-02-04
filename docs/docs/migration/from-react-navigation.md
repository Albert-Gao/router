---
title: From React Navigation
---

Prefer using index routes to setting `initialRouteName` in the navigator.

Use lowercase route names to be more consistent with the web: `Home` -> `home`.

Export named components as default:

```tsx
// Yes: This makes debugging easier and keeps Metro Fast Refresh working.
export default function Name() {
  return <View />;
}

// No: This breaks Fast Refresh.
export default function () {
  return <View />;
}
```

If you were passing complex options through the navigator, you should move to using [React context instead](https://reactnavigation.org/docs/hello-react-navigation#passing-additional-props).

### Navigators

| React Navigation             | Expo Router   |
| ---------------------------- | ------------- |
| `createStackNavigator`       | `Stack`       |
| `createBottomTabNavigator`   | `Tabs`        |
| `createDrawerNavigator`      | `Drawer`      |
| `createNativeStackNavigator` | `NativeStack` |

Expo Router wraps React Navigation APIs and re-exports them with light-weight context-aware wrappers.

```tsx
// No: This is a React Navigation API.
import { createStackNavigator } from "@react-navigation/stack";

const Stack = createStackNavigator();

// Yes: This is an Expo Router API.
import { Stack } from "expo-router";
```

### Link

Use `Link` from `expo-router` as this provides child context.

### Screen Options

> Think of this component like a `<head />` component from web frameworks.

Use `<[Layout].Screen />` in your screens to keep configuration in a single file. We don't currently bundle split as Metro doesn't support this yet, but we will in the future and self-contained configuration will lead to faster app startup.

```tsx
import { Tabs } from "expo-router";

export default function Home() {
  return (
    <View>
      <Tabs.Screen option={{ title: "Home" }} />
    </View>
  );
}
```
