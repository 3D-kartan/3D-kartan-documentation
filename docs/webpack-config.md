# Webpack config
This application (the frontend) uses webpack-5 to run in dev mode or build for production.

Most of the contents in the webpack file is pre configured. But some variables needs to be set:

```js
const apiBaseUrl = isProd
    ? "https://your-prod-url.se"
    : "http://localhost:4001";
```

The `devtool:` can also be set but for most use cases leave it to be:
```js
devtool: false
```

Other possible values are `source-map`.