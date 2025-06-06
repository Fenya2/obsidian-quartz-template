---
title: Higher-Order Layout Components
---

Quartz provides several higher-order components that help with layout composition and responsive design. These components wrap other components to add additional functionality or modify their behavior.

## `Flex` Component

The `Flex` component creates a [flexible box layout](https://developer.mozilla.org/en-US/docs/Web/CSS/flex) that can arrange child components in various ways. It's particularly useful for creating responsive layouts and organizing components in rows or columns.

```typescript
type FlexConfig = {
  components: {
    Component: QuartzComponent
    grow?: boolean // whether component should grow to fill space
    shrink?: boolean // whether component should shrink if needed
    basis?: string // initial main size of the component
    order?: number // order in flex container
    align?: "start" | "end" | "center" | "stretch" // cross-axis alignment
    justify?: "start" | "end" | "center" | "between" | "around" // main-axis alignment
  }[]
  direction?: "row" | "row-reverse" | "column" | "column-reverse"
  wrap?: "nowrap" | "wrap" | "wrap-reverse"
  gap?: string
}
```

### Example Usage

```typescript
Component.Flex({
  components: [
    {
      Component: Component.Search(),
      grow: true, // Search will grow to fill available space
    },
    { Component: Component.Darkmode() }, // Darkmode keeps its natural size
  ],
  direction: "row",
  gap: "1rem",
})
```

> [!note] Overriding behavior
> Components inside `Flex` get an additional CSS class `flex-component` that add the `display: flex` property. If you want to override this behavior, you can add a `display` property to the component's CSS class in your custom CSS file.
>
> ```scss
> .flex-component {
>   display: block; // or any other display type
> }
> ```

## `MobileOnly` Component

The `MobileOnly` component is a wrapper that makes its child component only visible on mobile devices. This is useful for creating responsive layouts where certain components should only appear on smaller screens.

### Example Usage

```typescript
Component.MobileOnly(Component.Spacer())
```

## `DesktopOnly` Component

The `DesktopOnly` component is the counterpart to `MobileOnly`. It makes its child component only visible on desktop devices. This helps create responsive layouts where certain components should only appear on larger screens.

### Example Usage

```typescript
Component.DesktopOnly(Component.TableOfContents())
```

## `ConditionalRender` Component

The `ConditionalRender` component is a wrapper that conditionally renders its child component based on a provided condition function. This is useful for creating dynamic layouts where components should only appear under certain conditions.

```typescript
type ConditionalRenderConfig = {
  component: QuartzComponent
  condition: (props: QuartzComponentProps) => boolean
}
```

### Example Usage

```typescript
Component.ConditionalRender({
  component: Component.Search(),
  condition: (props) => props.displayClass !== "fullpage",
})
```

The example above would only render the Search component when the page is not in fullpage mode.

```typescript
Component.ConditionalRender({
  component: Component.Breadcrumbs(),
  condition: (page) => page.fileData.slug !== "index",
})
```

The example above would hide breadcrumbs on the root `index.md` page.
