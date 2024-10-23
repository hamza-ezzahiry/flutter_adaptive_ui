# How to Build Adaptive UI with Flutter

This README summarizes the content of the Flutter I/O talk "How to build Adaptive UI with Flutter" presented by Tyler Holland and Reid Baker.  This talk focuses on building Flutter apps that adapt to different screen sizes, shapes, and input methods.

## Key Concepts

* **Adaptive UI:** Building apps that adjust their layout and functionality based on the device's characteristics.
* **Responsive Apps:** Apps that adapt to the size and shape of a user's device.
* **Foldables and Tablets:**  Considerations for building apps that work seamlessly on devices with foldable screens and larger displays.
* **Input Adaptation:** Handling different input types, such as touch, mouse, keyboard, and stylus.

## Handling Screen Cutouts and Notches

* **SafeArea:** Use the `SafeArea` widget to inset content and prevent it from being obscured by notches, cutouts, and system UI elements.  `SafeArea` intelligently manages padding and avoids stacking padding when nested.

```dart
SafeArea(
  child: YourContentWidget(),
)
```

## Adapting to Larger Screens

* **GridView:**  Transition from `ListView` to `GridView` for displaying content in a grid format, which is more suitable for larger screens.  `GridView.builder` is recommended for large datasets.
* **ConstrainedBox/Container with maxWidth:**  Restrict the maximum width of content to prevent it from stretching across the entire screen on larger devices. Material 3 recommends a maximum width of 840 logical pixels.

```dart
ConstrainedBox(
  constraints: const BoxConstraints(maxWidth: 840),
  child: GridView.builder(
    gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
      maxCrossAxisExtent: 250, // Adjust as needed
    ),
    // ... other GridView parameters
  ),
)
```

## Foldable Devices

* **Avoid Locking Screen Orientation:**  Locking screen orientation can lead to letterboxing issues on foldable devices.  Keep all orientations supported for optimal behavior.
* **New Display API (Flutter 3.13+):** If you must restrict orientation, use the new display API to get physical screen dimensions and avoid letterboxing.

```dart
// Get the display object for the current view
final display = displayOf(context);
final smallestSize = display.size.shortestSide;

// Use smallestSize to determine layout
```

## Dynamic Widgets and Layout

Use the "abstract, measure, branch" methodology:

1. **Abstract:** Identify common elements between different layouts and extract them into reusable widgets.

2. **Measure:** Determine the appropriate method for obtaining size information:
    * **MediaQuery.sizeOf(context):** Use for decisions based on the overall app window size. Rebuilds the widget when the size changes.
    * **LayoutBuilder:** Use for decisions based on the available space within a specific part of the layout. Provides BoxConstraints.

3. **Branch:** Implement conditional logic to switch between different layouts based on the measured size.  Refer to Material Design guidelines for breakpoints (e.g., 600 logical pixels).

| Feature | Code Example |
|---|---|
| Dialogs (Fullscreen vs. Modal) | See code example in transcript around timecode 17:00 |
| Navigation (BottomNavigationBar vs. NavigationRail) | See code example in transcript around timecode 20:00 |
| Custom Layout (Vertical vs. Horizontal) | See code example in transcript around timecode 23:00 |


## Input Adaptation

* **Material Widgets:**  Built-in Material widgets automatically handle different input states (hover, focus, etc.).
* **FocusableActionDetector:** For custom widgets, use `FocusableActionDetector` to add interactivity for hover, focus, keyboard, and D-pad navigation.

```dart
FocusableActionDetector(
  onShowHoverHighlight: (isHovered) { /* Update UI */ },
  onFocusChange: (isFocused) { /* Update UI */ },
  // ... other parameters
  child: YourWidget(),
)
```

## Best Practices

* **Don't lock orientation.**
* **Break down complex widgets into smaller, reusable components.**
* **Avoid checking device type; use MediaQuery or LayoutBuilder instead.**
* **Don't use Orientation or OrientationBuilder for layout decisions; use size instead.**
* **Use logical pixels for measurements.**
* **Consider Material Design breakpoints (600, 840 logical pixels).**

## Further Resources

* Flutter Channel on YouTube
* [flutter.dev/adaptive](flutter.dev/adaptive)
* "Building an Animated Responsive App Layout with Material 3" codelab
* "Flutter Lessons for Federated Plugin Development" (Flutter I/O talk)
* Wonderous app on GitHub (example of adaptive Flutter app)
* m3.material.io (Material 3 Layout Guidance)
