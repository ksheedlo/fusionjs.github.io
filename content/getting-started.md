# Getting started

### Set up Fusion.js packages

Fusion.js lets you customize virtually all aspects of an application. In this example, we'll see how to set up a React-based application.

First, let's install the packages we'll need:

```sh
yarn add fusion-core fusion-cli fusion-react fusion-react-async react react-dom
```

Next, let's add scripts to `package.json`:

```json
{
  "scripts": {
    "dev": "fusion dev",
    "test": "fusion test",
    "build": "fusion build",
    "start": "fusion start"
  }
}
```

---

### Hello world

Fusion expects the entry file to be at `src/main.js`. There we can specify what rendering library we want to use. For convenience, the `fusion-react` package provides an entry-point application class that is already configured to work with React. Let's use that:

```js
// src/main.js
import App from 'fusion-react';
```

The `App` class constructor takes a React element. This is the root element of the application:

```js
new App(<div>Hello world</div>);
```

Now that we've configured our application, we just need to export a function that returns it:

```js
// src/main.js
import App from 'fusion-react';
import React from 'react';

export default () => {
  return new App(<div>Hello world</div>);
};
```

To run the application, run this command from your CLI:

```sh
yarn run dev
```

The application will be available at `http://localhost:3000` and will render `Hello world`.

Try changing the text to see hot reloading in action.

While the Fusion.js CLI takes care of developer productivity concerns such as Babel configuration, build-time orchestration and hot module reloading, the Fusion.js runtime does very little. This ensures that the baseline build of a Fusion.js app is lean and flexible.

However, apps can gain more functionality via plugins. In the next section, we'll look at how to use a plugin.

---

### Styling

Let's install these packages:

```sh
yarn add fusion-plugin-styletron-react styletron
```

This package contains the plugin for Styletron, which, in addition to providing an easy-to-use styled-component-like interface, provides [powerful server-side CSS optimizations](https://ryantsao.com/blog/virtual-css-with-styletron), yielding less CSS code down the wire.

To register the plugin, let's modify `src/main.js` to register the Styletron plugin:

```js
// src/main.js
import App from 'fusion-react';
import Styletron from 'fusion-plugin-styletron-react';
import React from 'react';

export default () => {
  const app = new App(<div>Hello world</div>);

  app.register(Styletron);

  return app;
};
```

Now, let's move our `<div>` element to a separate file called `src/components/root.js` and replace the div with a styled one:

```js
// src/components/root.js
import React from 'react';
import {styled} from 'fusion-plugin-styletron-react';

const Panel = styled('div', {background: 'silver'});

export default <Panel>Hello</Panel>;

// src/main.js
import App from 'fusion-react';
import Styletron from 'fusion-plugin-styletron-react';

import root from './components/root';

export default () => {
  const app = new App(root);

  app.register(Styletron);

  return app;
}
```

The application in your browser should automatically reload to display the silver background.

---

### Font loading

Fonts can be loaded via the `fusion-plugin-font-loader-react` plugin:

```js
// src/main.js
import App from 'fusion-react';
import Fonts, {
  FontLoaderReactConfigToken
} from 'fusion-plugin-font-loader-react';
import {assetUrl} from 'fusion-core';
import root from './components/root';

export default () => {
  const app = new App(root);

  app.register(Fonts);
  app.register(FontLoaderReactConfigToken, {
    preloadDepth: 1,
    fonts: {
      'Lato-Regular': {
        urls: {
          woff: assetUrl('../static/Lato-Regular.woff'),
          woff2: assetUrl('../static/Lato-Regular.woff2'),
        },
        fallback: {
          name: 'Helvetica',
        },
      }
    }
  });

  return app;
}
```

---

### Assets

Use the virtual module `assetUrl` for other asset types, such as images.

```js
// src/components/root.js
import React from 'react';
import {styled} from 'fusion-plugin-styletron-react';
import {assetUrl} from 'fusion-core';

const Panel = styled('div', {background: 'silver'});

export default (
  <Panel>
    <img src={assetUrl('../../static/image.gif')} />
  </Panel>
);
```

Note that the argument to `assetUrl` needs to be a compile-time static string literal.

---

### Next steps

Here are some more in-depth sections covering various aspects of Fusion.js:

#### Core concepts

* [Universal code](universal-code.md)
* [Creating a plugin](creating-a-plugin.md)
  * [Dependencies](dependencies.md)
  * [Creating endpoints](creating-endpoints.md)
  * [Creating providers](creating-providers.md)
  * [Modifying the HTML template](modifying-html-template.md)
  * [Working with secrets](working-with-secrets.md)

#### Plugins

Check out the links below to help you get familiar with other useful plugins that are provided by the Fusion.js team:

* [Styletron](https://github.com/fusionjs/fusion-plugin-styletron-react)
* [React Router](https://github.com/fusionjs/fusion-plugin-react-router)
* [RPC/Redux](https://github.com/fusionjs/fusion-plugin-rpc-redux-react)
* [I18n](https://github.com/fusionjs/fusion-plugin-i18n-react)
* [Error handling](https://github.com/fusionjs/fusion-plugin-error-handling)
* [Logging](https://github.com/fusionjs/fusion-plugin-universal-logger)
