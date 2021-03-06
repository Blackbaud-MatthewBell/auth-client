# @blackbaud/auth-client

[![npm](https://img.shields.io/npm/v/@blackbaud/auth-client.svg)](https://www.npmjs.com/package/@blackbaud/auth-client)
[![status](https://travis-ci.org/blackbaud/auth-client.svg?branch=master)](https://travis-ci.org/blackbaud/auth-client)
[![coverage](https://codecov.io/github/blackbaud/auth-client/coverage.svg?branch=master)](https://codecov.io/github/blackbaud/auth-client/)

Provides a client-side library for interacting with Blackbaud authentication.

## Installation

- Ensure that you have Node v6+ and NPM v3+. To verify this, run `node -v` and `npm -v` at the command line.
- Install the library as a dependency of your project by running `npm install @blackbaud/auth-client --save` in your project's folder.

## Usage

### ES6/TypeScript

There are two classes available in this package: `BBAuth` and `BBOmnibar`.  `BBAuth` allows you to retrieve an auth token from the Blackbaud authentication service, and `BBOmnibar` allows you to render the omnibar at the top of the page.

You can use these in combination to integrate your application with Blackbaud authentication.

```
import { BBAuth, BBOmnibar } from '@blackbaud/auth-client';

// Make an initial attempt to get an auth token.  If the user is not currently logged in,
// this code will redirect the browser to Blackbaud's sign-in page.
BBAuth.getToken()
  .then(() => {
    // The user is logged in; load the omnibar.
    BBOmnibar.load({
      serviceName: 'Some service name'
    });

    // Add additional logic to bootstrap the rest of the application.
  });
```

To make authorized requests to your web service endpoints you will also use the `BBAuth.getToken()` method to retrieve a token that can be added as a header to your request.  Since retrieving a token is an asynchronous operation, this method returns a `Promise`, so you should wait until the Promise is resolved before making your web request.

```
import { BBAuth } from '@blackbaud/auth-client';

BBAuth.getToken()
  .then((token: string) => {
    const xhr = new XMLHttpRequest();

    xhr.open('GET', url, true);

    xhr.setRequestHeader('Authorization', 'Bearer ' + token);

    xhr.send();
  });
```

### Vanilla JavaScript/ES5

Auth client is also distributed as a UMD bundle.  If you're using ES5 with Node or a tool like Browserify you can `require()` it:

```
var BBAuthClient = require('@blackbaud/auth-client');

BBAuthClient.BBOmnibar.load({
  serviceName: 'Some service name'
});
```

If you're using no module loader at all, then you can load the `dist/bundles/auth-client.umd.js` file onto your page and via a `<script>` element or concatenated with the rest of your page's JavaScript and access it via the global `BBAuthClient` variable:

```
// BBAuthClient is global here.
BBAuthClient.BBOmnibar.load({
  serviceName: 'Some service name'
});
```
