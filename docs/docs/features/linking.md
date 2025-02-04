---
title: Linking
sidebar_position: 2
---

:::warning Beta Release

Expo Router is in the earliest stage of development. The API is subject to breaking changes. The documentation is incomplete and may be inaccurate. The project is not yet ready for production use. Please [contribute to the discussion](https://github.com/expo/router/discussions/1) if you have any ideas or suggestions on how we can improve the convention.

:::

The `expo-router` `Link` component supports client-side navigation to a route. It is similar to the `Link` component in `react-router-dom` and `next/link`.

When JavaScript is disabled or the client is offline, the `Link` component will render a regular `<a>` element. Otherwise, the default behavior will be intercepted and the client-side router will navigate to the route (faster and smoother).

Meaning you get the best of both worlds: a fast client-side navigation experience, and a fallback for when JavaScript is disabled or hasn't loaded yet.

```tsx
import { Link } from "expo-router";

export default function Page() {
  return (
    <View>
      <Link href="/">Home</Link>
    </View>
  );
}
```

Try to use the `Link` component whenever possible as it renders the correct semantic HTML element on web. This is important for accessibility and SEO.

## Imperative API

For more advanced use cases, you can use the imperative `useLink` hook to navigate imperatively.

```js
import { View, Text } from "react-native";
import { useLink } from "expo-router";

export default function Page() {
  const link = useLink();

  return (
    <View>
      <Text onPress={() => link.push("/profile/settings")}>Settings</Text>
    </View>
  );
}
```

Prefer the `useLink` hook to `useNavigation` from React Navigation as `useLink` works much closer to the `<Link />` component.

## `useLink` API

- **push**: _`(href: Href) => void`_ Navigate to a route. You can provide a full path like `/profile/settings` or a relative path like `../settings`. Navigate to dynamic routes by passing an object like `{ pathname: 'profile', query: { id: '123' } }`.
- **back**: _`() => void`_ Navigate to a route. You can provide a full path like `/profile/settings` or a relative path like `../settings`. Navigate to dynamic routes by passing an object like `{ pathname: 'profile', query: { id: '123' } }`.

> This API is similar to Next.js's `useRouter` hook, but adjusted to work around the state management requirements of native.

## `Href` type

The `Href` type is a union of the following types:

- **string**: A full path like `/profile/settings` or a relative path like `../settings`.
- **object**: An object with a `pathname` and optional `query` object. The `pathname` can be a full path like `/profile/settings` or a relative path like `../settings`. The `query` can be an object of key/value pairs.

## Navigation prop

You can also use the [`navigation` object](https://reactnavigation.org/docs/navigation-prop) from React Navigation to imperatively navigate using screen names:

```js
export default function Route({ navigation }) {
  return (
    <View>
      <Text
        onPress={() => {
          // Go back to the previous screen using the imperative API.
          navigation.goBack();
        }}
      >
        Details Screen
      </Text>
    </View>
  );
}
```

Alternatively, you can access the `navigation` prop from any component using the hook:

```js
import { useNavigation } from "@react-navigation/native";

function MyBackButton() {
  const navigation = useNavigation();

  return <Button title="Go back" onPress={() => navigation.goBack()} />;
}
```

The navigation prop is useful for layout-specific functionality like `navigation.openDrawer()` in a Drawer layout. [Learn more](https://reactnavigation.org/docs/navigation-prop#navigator-dependent-functions).

## Testing

On native, you can use the `uri-scheme` CLI to test opening native links on a device.

```bash
npx uri-scheme open exp://192.168.87.39:19000/--/form-sheet --ios
```

You can also search for links directly in a browser like Safari or Chrome to test deep linking on physical devices. Learn more about [testing deep links](https://reactnavigation.org/docs/deep-linking).

## Dev mode

![](/img/directory.png)

We currently inject a `/__index` file which provides a list of all routes in the app. This is useful for debugging and development. This screen is only injected during development, and won't be available in production.

We may remove this feature for the official release, but it's useful for now.
