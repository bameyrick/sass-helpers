# sass-helpers

A library of helpers and mixins for helping with building responsive stylesheets.

## Helpers

### Breakpoints
---
#### add-breakpoint
Defines a breakpoint to be called using the breakpoint mixin at a later point.

| Argument    | Type   | Optional | Example        |
| ----------- | ------ | -------- | -------------- |
| $name       | string | false    | `mobile`       |
| $dimensions | string | false    | `768px 1024px` |

##### Usage
```scss
$mobile: 420px;
$tablet: 768px;
$tablet-landscape: 1024px;
$desktop: 1366px;
$desktop-large: 1560px;

@include add-breakpoint('mobile', 0px $mobile);
@include add-breakpoint('mobile-large', $mobile $tablet);
@include add-breakpoint('tablet', $tablet $tablet-landscape);
@include add-breakpoint('tablet-landscape', $tablet-landscape $desktop);
@include add-breakpoint('desktop', $desktop $desktop-large);
@include add-breakpoint('desktop-large', $desktop-large 99999px);
```
---
#### breakpoint
Generates a breakpoint and wraps your rule declaration or properties with it.

| Argument   | Type   | Optional | Example |
| ---------- | ------ | -------- | ------- |
| $name      | string | false    | `mobile`|
| $direction | string | false    | `up`    |

##### Usage
```scss
@include breakpoint(tablet, down) {
  .cls {
    background: red;
  }
}

.cls-2 {
  @include breakpoint(tablet, up) {
    color: green;
  }
  
  @include breakpoint(tablet, below) {
    color: purple;
  }
}

@include breakpoint(tablet-portrait, above) {
  .cls-3 {
    text-decoration: underline;
  }
}
```

###### will render:
```css
@media (max-width: 1023px) {
  .cls {
    background: red;
  }
}

@media (min-width: 768px) {
  .cls-2 {
    color: green;
  }
}

@media (max-width: 767px) {
  .cls-2 {
    color: purple;
  }
}

@media (min-width: 1367px) {
  .cls-3 {
    text-decoration: underline;
  }
}
```
---

### Fluid Properties
These helpers will output calculations and clamping breakpoints to blend a CSS property between two defined points, and two clamping positions.

---
#### fluid-property

| Argument            | Type    | Optional | Default      | Example      |
| ------------------- | ------- | -------- | ------------ | ------------ |
| $property           | string  | false    | n/a          | `margin-top` |
| $min                | string  | false    | n/a          | `10px`       |
| $max                | string  | false    | n/a          | `20px`       |
| $clip-low           | string  | true     | `420px`      | `768px`      |
| $clip-high          | string  | true     | `1366px`     | `1024x`      |
| $clip               | boolean | true     | `true`       | `false`      |
| $clip-at-start      | boolean | true     | `true`       | `true`       |
| $clip-at-end        | boolean | true     | `true`       | `true`       |
| $viewport-direction | string  | true     | `horizontal` | `vertical`   |

##### Usage
```scss
.cls {
  @include fluid-property(margin-top, 10px, 20px);
  @include fluid-property(padding-bottom, 5px, 20px, $clip-at-start: false, $viewport-direction: 'vertical');
}
```

###### will render:
```css
.cls {
  margin-top: calc(10px + 10 * ((100vw - 420px) / 946));
  padding-bottom: calc(5px + 15 * ((100vh - 420px) / 946));
}

@media (max-width: 420px) {
  .cls {
    margin-top: 10px; 
  }
}

@media (min-width: 1366px) {
  .cls {
    margin-top: 20px;
  }
}

@media (min-height: 1366px) {
  .cls {
    padding-bottom: 20px; 
  } 
}
```
