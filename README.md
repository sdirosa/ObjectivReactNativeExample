# ObjectivReactNativeExample
A clean expo React Native install with Objectiv tracker configured

## Changes
navigation/index.tsx has been modified from:

```tsx
export default function Navigation({ colorScheme }: { colorScheme: ColorSchemeName }) {
  return (
    <NavigationContainer
      linking={LinkingConfiguration}
      theme={colorScheme === 'dark' ? DarkTheme : DefaultTheme}>
      <RootNavigator />
    </NavigationContainer>
  );
}
```


to:

```tsx
import { ContextsFromReactNavigationPlugin } from "@objectiv/plugin-react-navigation";
import { ObjectivProvider, ReactNativeTracker } from "@objectiv/tracker-react-native";
import { DebugTransport } from "@objectiv/transport-debug";
```

```tsx
export default function Navigation({ colorScheme }: { colorScheme: ColorSchemeName }) {
  const navigationContainerRef = useNavigationContainerRef();

  const tracker = new ReactNativeTracker({
    applicationId: 'native-react-test',
    transport: new DebugTransport(),
    plugins: [
      new ContextsFromReactNavigationPlugin({ navigationContainerRef })
    ]
  })

  return (
    <ObjectivProvider tracker={tracker}>
      <NavigationContainer
        linking={LinkingConfiguration}
        theme={colorScheme === 'dark' ? DarkTheme : DefaultTheme}
        ref={navigationContainerRef}
      >
        <RootNavigator />
      </NavigationContainer>
    </ObjectivProvider>
  );
}
```
