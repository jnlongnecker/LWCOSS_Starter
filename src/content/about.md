# About LWR

LWR stands for _Lightning Web Runtime_. This is the environment that Lightning Web Components run in. By running `npm init lwr@latest`, we created a project that contains the LWR. The `npm install` command installs the other node modules that LWR depends on to properly function, like `Express` and LWC modules.

---

To explain another common acronym that you'll see, OSS stands for _Open Source Software_. I'll be using this to differentiate the LWC we'll be using here from the platform LWC we use on Salesforce; as there are a few differences.

## Routing and Layouts

Our project is a fully-functioning website, complete with a server and arbitrary number of pages. Our access to the server is cut-off (unless you know what you're doing) and the primary way we interface with it is through the `lwr.config.json` file.

Here, you will find a `JSON` file with a few nodes of interest:

-   `lwc` : This is a route to the component modules
-   `routes` : This is a series of objects that represent a web route and what is found there
-   `assets` : This is a shorthand for asset files/directories
-   `moduleProviders` : This is brand new and haven't seen documentation on what it does yet

Focusing in on the `routes` node, this is where we configure the url paths for our website. There is an absurd amount of things to talk about around this topic; far too much for me to go into detail on. One thing to mention is that each route must have one of two properties: `layoutTemplate` or `routeHandler`. We won't talk about `routeHandler` as it's rather complex, but instead we'll focus on the first one.

### `layoutTemplate`

The `layoutTemplate` is a string with a path to a particular `.njk` or Nunjucks file. If you go and open up one of these files, it will initially appear to be very similar to a regular HTML file. However, one of the differences of this file is it uses a framework called Nunjucks to allow you to inject data into the HTML file from the server side.

Using Nunjucks is again too large a topic for me to cover in any brevity, so I will instead share with you the [Nunjucks documentation](https://mozilla.github.io/nunjucks/templating.html). What you will want to know are these bits of information:

-   To inject the Markdown content defined with the `contentTemplate` of the route, write `{{ body | Safe }}`
-   To allow the usage of LWC on the page, write `{{ lwr_resources | safe }}`
    -   Once you have this on the template, you can use LWC as you would normally
-   To inject data defined in a `properties` node of the route, use `{{ nodeName }}`
