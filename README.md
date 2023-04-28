# Static Site Boilerplate Example

The **Static Site** example contains the minimum code needed to get up and running with a LWR website.

## Project Setup

The directory structure looks like this:

```
src/
  ├── assets/           // static assets
  |   └── favicon.ico
  │   └── css/
  │       └── main.css
  └── content/          // site pages
  │   ├── about.md
  │   └── home.md
  └── layouts/          // site page layouts
      └── main_layout.njk
lwr.config.json         // lwr configuration
package.json            // npm packaging configuration
```

## Configuration

The LWR server is configured in `lwr.config.json`, at the root of the project. The **Static Site** example has two routes/pages.

```json
// lwr.config.json
{
    "lwc": { "modules": [{ "dir": "$rootDir/src/modules" }] },
    "routes": [
        {
            "id": "Home",
            "path": "/",
            "contentTemplate": "$contentDir/home.md",
            "layoutTemplate": "$layoutsDir/main_layout.njk"
        },
        {
            "id": "About",
            "path": "/about",
            "contentTemplate": "$contentDir/about.md",
            "layoutTemplate": "$layoutsDir/main_layout.njk"
        }
    ],
    "assets": [
        {
            "alias": "assetsDir",
            "dir": "$rootDir/src/assets",
            "urlPath": "/public/assets"
        },
        {
            "alias": "favicon",
            "file": "$rootDir/src/assets/favicon.ico",
            "urlPath": "/favicon.ico"
        }
    ]
}
```

## Running the Project in dev Mode

```bash
yarn install
yarn dev
```

Open the site at [http://localhost:3000](http://localhost:3000)

## Statically Generate and Preview the Site

```bash
yarn build
yarn start
```

Open the site at [http://localhost:3000](http://localhost:3000)
