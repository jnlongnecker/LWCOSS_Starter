# Components and Custom Server

In order to use components, we need to create a `modules` folder. This is the default name, but we can change the directory that LWR looks for component modules in by changing the `lwc` node in the `lwr.config.json` file. In here, we can then create namespace folders to contain our component bundles in.

For example, if you had a folder structure that looked like this:

```
modules
|    c
|    |    demo
|    input
|    |    button
|    |    switch
```

You would have the following components:

-   `c-demo`
-   `input-button`
-   `input-switch`

LWC OSS is the first time where we can fully realize the namespace system of LWC without the use of managed packages. The features in the components are largely the same as the ones you find in platform LWC. What is specifically available changes over time and is rapidly changing faster than the platform LWC, so I won't attempt to list them. You'll have to check the documentation!

The main difference that will remain consistent is the lack of the meta file in the component bundle. Since this is also not an SFDX project, we also cannot make use of SFDX tools to create LWC bundles easily. I've not checked thoroughly enough to see if there's an extension that does this for OSS LWC, but feel free to check. We also won't be able to make use of `@salesforce` modules as well since we're not on the platform. This _doesn't_ mean we cannot communicate with a Salesforce org; only that the way we would do so is different (and a bit more difficult).

Converting a component from platform to OSS LWC is therefore rather difficult, but converting a component from OSS to platform LWC is much easier. To convert to a platform LWC, all we need to do is add the meta file and remove any experimental features not available to platform LWC.

Converting from platform to OSS is likely to be much more difficult due to the fact that platform LWCs almost certainly make use of Salesforce data. We'll need to first remove the meta file, but once that is done we'll have to completely re-write our component to interface with Salesforce in a different way, which is outside of what this will cover.

While I won't be covering specifically how to do it, there are a number of standard LWCs that are implement for OSS LWC that you can use, and you can additionally make use of SLDS as well with a bit of configuration.

## Custom Server Functionality with Node.js

Node.js is again an exceptionally large topic that I won't hope to be able to cover, however I will show some code that can be used to construct your own wrapper around the server running LWR to allow you to customize it with code.

```js
// startServer.cjs

// Get a reference to the lwr module that holds the server reference
const lwr = require("lwr");

// Build the server running LWR
const lwrServer = lwr.createServer();

// Get a reference to the server, allowing you to customize responses to HTTP requests
const app = lwrServer.getInternalServer("express");

/*
    ... Customize HTTP routes here ...
*/

// Set mode to dev mode (for clearer debugging) by default
// If the user inputs the mode, overwrite with that
let mode = "dev";
if (process.argv.length >= 3) {
    mode = process.argv[2];
}
lwrServer.config.serverMode = mode;

// Start the server
lwrServer
    .listen(({ port, serverMode }) => {
        console.log("Server started");
    })
    .catch((err) => {
        console.error(err);
        process.exit(1);
    });
```

With this, you'll have to make sure that you use this file to start your server. You'll likely now use the command `node startServer.cjs`, where `startServer.cjs` is the name of the file containing the code above. If the `startServer.cjs` file is in another directory from your current terminal directory, you'll have to make sure that this is a path to this file.

You can customize your `package.json` file to allow you to use the command `npm run dev` again, or even use a command of your own choosing. Again, outside the context of what I want to explain here.
