# @cap.js/widget

> [!NOTE]  
> **Requirements:** All modern browsers should be supported, but the build script specifically targets the last 10 versions of Chrome, Firefox, Safari and Edge.

`@cap.js/widget` is Cap's client-side library. It includes the `cap-widget` web component, the invisible mode and the Captcha solver. First, add it to your client-side code:

::: code-group

```html [jsdelivr]
<script src="https://cdn.jsdelivr.net/npm/@cap.js/widget"></script>
```

```html [unpkg]
<script src="https://unpkg.com/@cap.js/widget"></script>
```

```html [standalone server]
<script src="https://<server url>/assets/widget.js"></script>

<!-- tip: make sure to set the wasm endpoint too! -->
```

:::

::: warning  
We're using the latest version of the library here for simplicity, but you should optimally pin a specific version to avoid breaking changes in the future.  
:::

You can now use the `<cap-widget>` component in your HTML.

```html
<cap-widget id="cap" data-cap-api-endpoint="<your cap endpoint>"></cap-widget>
```

> [!NOTE]  
> You'll need to start a server with an API exposing the Cap methods running at the same URL as specified in the `data-cap-api-endpoint` attribute.  

> [!TIP] The following attributes are supported:
>
> - `data-cap-api-endpoint`: API endpoint (required)
> - `data-cap-worker-count`: Number of workers to use (defaults to `navigator.hardwareConcurrency || 8`)

Then, in your JavaScript, listen for the `solve` event to capture the token when generated:

```js{3}
const widget = document.querySelector("#cap");

widget.addEventListener("solve", function (e) {
  const token = e.detail.token;

  // Handle the token as needed
});
```

Alternatively, you can use `onsolve=""` directly within the widget or wrap the widget in a `<form></form>` (where Cap will automatically submit the token alongside other form data. for this, it'll create a hidden field with name set to its `data-cap-hidden-field-name` attribute or `cap-token`).

## Supported events

The following custom events are supported:

- `solve`: Triggered when the token is generated.
- `error`: Triggered when an error occurs.
- `reset`: Triggered when the widget is reset.
- `progress`: Triggered when there's a progress update while in verification.

## i18n

You can change the text on each label of the widget by setting the `data-cap-i18n-*` attribute, like this:

```html
<cap-widget
  id="cap"
  data-cap-api-endpoint="<your cap endpoint>"
  data-cap-i18n-verifying-label="Verifying..."
  data-cap-i18n-initial-state="I'm a human"
  data-cap-i18n-solved-label="I'm a human"
  data-cap-i18n-error-label="Error"
></cap-widget>
```

## Custom fetch

You can override the default fetch implementation by setting `window.CAP_CUSTOM_FETCH` to a custom function. This function will receive the URL and options as arguments and should return a promise that resolves to the response.

```js
window.CAP_CUSTOM_FETCH = function (url, options) {
  // Custom fetch implementation
  return fetch(url, options);
};
```

## Custom WASM URL

You can override the default WASM URL by setting `window.CAP_CUSTOM_WASM_URL` to a custom URL. This URL will be used to load the WASM module. This defaults to `https://cdn.jsdelivr.net/npm/@cap.js/wasm@0.0.4/browser/cap_wasm.min.js`
