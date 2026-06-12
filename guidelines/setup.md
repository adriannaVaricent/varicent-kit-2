## Package Installation

**CRITICAL**: You MUST explicitly install every package below as a direct dependency, exactly as written. Transitive availability does NOT count.

```json
{
  "dependencies": {
    "@varicent/varicent-ui-core": "8.4.0",
    "@varicent/varicent-ui-icons": "8.4.0",
    "@varicent/varicent-ui-illustrations": "8.4.0",
    "@varicent/varicent-ui-tokens": "8.4.0",
    "@emotion/react": "11.14.0",
    "@emotion/styled": "11.14.1",
    "react-intl": "7.1.14"
  }
}
```

## Required CSS import

In your app entry-point CSS file:

```css
@import "@varicent/varicent-ui-tokens/dist/variables.css";
```

This makes all `var(--token-name)` CSS custom properties available globally.

## Required React providers

Varicent UI components call `useIntl()` internally — without `IntlProvider` everything throws:

```tsx
import { IntlProvider } from "react-intl";
import { intlMessages } from "@varicent/varicent-ui-core";

export default function App() {
  return (
    <IntlProvider locale="en" defaultLocale="en" messages={intlMessages}>
      {/* rest of app */}
    </IntlProvider>
  );
}
```

## Emotion `css` prop

Any file that applies the `css` prop to a **raw DOM element** (`<div>`, `<span>`, etc.) needs this pragma at the very top:

```tsx
/** @jsxImportSource @emotion/react */
import { css } from "@emotion/react";
```

Design-system components (`Button`, `Card`, `LayoutColumn`, etc.) handle this internally — you only need the pragma in files that use `css` on raw HTML elements. **Never** apply `css` to raw DOM without the pragma — it throws at runtime.

## Vite config

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [
    react({
      jsxImportSource: "@emotion/react",
      babel: {
        plugins: ["@emotion/babel-plugin"],
      },
    }),
  ],
});
```
