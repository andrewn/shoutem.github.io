---
layout: doc
permalink: /docs/extensions/reference/extension
title: Extension file format
section: Reference
---

# Extension file format

The main file that describes every extension is `extension.json`, which is located in the root folder of the extension.

## Structure of extension.json

Following structure shows only `root` fields of the extension.json. Detailed description about each of those fields is [below](#fields).

```json
{
  // required
  "shoutem": "1.0",
  "name": "restaurants",
  "version": "0.0.1",

  // recommended
  "title": "Restaurants",
  "description": "List restaurants in your app",
  "website": "https://www.shoutem.com/restaurants",
  "icon": "server/assets/extension/icon.png",

  // optional
  "defaultLocale": "en",
  "settingsPages": [{...}],
  "settings": {...},

  // optional exports (extension parts)
  "shortcuts": [{...}],
  "screens": [{...}],
  "dataSchemas": [{...}],
  "pages": [{...}],
  "themes": [{...}],
  "variablesSchemas": [{...}]
}
```

## Identifying and referencing extension parts

As you see in _Structure of extension.json_ chapter, extension exports multiple extension parts (shortcuts, screens, dataSchemas, pages, themes, variablesSchemas). In order to be able to use these extension parts, we need to identify them, so we can later reference them in other parts. Identifying is done with `name` field which value needs to be unique for that extension part (`RestaurantsList` can be only used for 1 shortcut, but also for 1 screen, etc.). 

On the other hand, when referencing extension parts, fully qualified name needs to be used. Fully qualified name of extension is done by prefixing `developerName.` to `name` field (for above example written by Shoutem as developer, extension would have identity of `shoutem.restaurants`). Fully qualified name of extension parts is done by suffixing `developerName.extensionName.` with the unique identifier for that extension part, e.g. `shoutem.restaurants.RestaurantsList` for shortcut. Shorthand for `developerName.extensionName.` prefix is `@.`, so we can reference it with `@.RestaurantsList` instead.


## Fields

Here you can find field explanations in the same order fields appeared in the upper example:

#### shoutem

Required field. Indicates version of Shoutem extension standard. There's currently only `"1.0"` standard.

#### name

Required field. Defines extension's identity. Must be unique among your extensions and not longer than 32 characters.

#### version

Version of your extension.

#### title

Title of your extension.

#### description

Description of your extension.

#### website

Website showing your extension.

#### icon

Path to extension's icon that will be present in Shoutem Extension Market. Store the icon in `server` asset's folder, as it will be used on Shoutem's server side.

#### defaultLocale

Default locale of your extension. Check [localization](/docs/coming-soon) on how to localize your extension.

#### settingsPage

Array of [extension pages](/docs/coming-soon) used to manage the global settings of the extension.

```json
[{
  // required
  "page": "@.Settings",

  // recommended
  "title": "Settings",

  // optional
  "parameters": {
    "any-parameter": "any-value"  
  }
}]
```

Each object in settings pages array, settings page object, consist of these fields:

- `page`: Required field, references the extension page
- `title`: Title of extension page
- `parameters`: Dictionary of arbitrary key/value pairs that will be passed to admin page instance

#### settings

Dictionary of arbitrary key/value pairs that represent default extensions's settings passed to settings pages objects.

```json
{
  "any-parameter": "any-value"  
}
```

#### shortcuts

[Shortcuts](/docs/extensions/getting-started/shortcut-and-screen) are links to the starting screen of your extension. Format:

```json
[{
  // required
  "name": "RestaurantsList",

  // required (pick one)
  "screen": "@.RestaurantsList",
  "action": "visitRestaurants"

  // recommended
  "title": "Restaurants",
  "description": "Allow users...",
  "iconUrl": "server/assets/shortcuts/restaurants-list.png",

  // optional
  "type": "navigation",
  "adminPages": [{
    // required
    "page": "@.CmsPage",

    // recommended
    "title": "Content",

    // optional
    "parameters": {
      "schema": "@.Restaurants"
    },
  }],
  "settings": {
    "any-parameter": "any-value"
  }
}]
```

Each object in shortcuts array, shortcut object, consists of these fields:

- `name`: Required field, defines shortcut's identity
- `screen/action`: Shortcut can either open a `Screen` or call an `Action`
- `title`: Shortcut's title
- `description`: Shortcut's description
- `iconUrl`: Path to shortcut's icon that will be shown in builder. Store in `server` asset's folder
- `type`: Indicates the type of shortcut. It can be `navigation` or `undefined`. If `navigation`, it will be possible to nest other shortcuts below the current
- `adminPages`: Array of shortcut's admin pages. Admin page object inside of array consists of:
  - `page`: Required field, references an [extension page](/docs/coming-soon)
  - `title`: Title of admin page
  - `parameters` Dictionary of arbitrary key/value pairs that will be passed to admin page instance
- `settings`: Dictionary of arbitrary key/value pairs that represent default Shortcut's settings passed to admin pages


#### screens

Screens are nothing more than React components which represent full mobile screen. Format:

```json
[{
  // required
  "name": "List",

  // recommended
  "title": "List",
  "image": "server/assets/screens/restaurants-list.png",
  
  // optional
  "navigatesTo": [{
    "details": "@.Details"
  }],
  "settingsPage": {
    // required
    "page": "@.List",

    // optional
    "parameters": {
      "any-parameter": "any-value"  
    }
  },
  "settings": {
    "any-parameter": "any-value"
  }
}, {
  "name": "Grid",
  "title": "Grid",
  "image": "server/assets/screens/restaurants-grid.png",
  "extends": "@.List",
  "settingsPage": {
    "page": "@.List",
    "parameters": {
      "any-parameter": "any-value"  
    }
  },
}]
```

Each object in screens array, screen object, consists of these fields:

- `name`: Required field, defines screen's identity
- `title`: Screen's title that will be shown in [layout selector](/docs/coming-soon)
- `image`: Path to screen's image that shows it's layout
- `navigatesTo`: Array of key/value pairs that indicates to which screens the current one can navigate to
- `settingsPage`: Screen's settings page. Object consists of:
  - `page`: Required field, references an [extension page](/docs/coming-soon)
  - `parameters`: Dictionary of arbitrary key/value pairs that will be passed to settings page instance
- `settings`: Dictionary of arbitrary key/value pairs that represent default Shortcut's settings passed to admin pages
- `extends`: References screen that the current one is extending

In the example above, we included 2 screen objects inside of the `screens` array. We wanted to show you the usage of `extends` field. Extending makes it possible to [switch between multiple alternative layouts](/docs/extensions/tutorials/alternativelayouts).


#### dataSchemas

[Data Schemas](/docs/extensions/getting-started/using-cloud-storage) are Shoutem-flavored [JSON Schemas](http://json-schema.org/) which describe data stored on Shoutem's CMS.

```json
[{
  // required
  "name": "Restaurants",
  "path": "server/data-schemas/restaurants.json"
}]
```

Each object in data schemas array, data schema object, consists of these fields:

- `name`: Required field, defines data schema's identity
- `path`: Required field, path to actual schema implementation. Should be stored in `server` folder


#### pages

[Extension pages](/docs/coming-soon) are Webpages that extension can use for multiple extension parts:

- global settings of the extensions (referenced from `settingsPages` in the root of extension.json)
- settings of the shortcut (referenced from `adminPages` in the shortcut object)
- settings of the screen (referenced from `settingsPage` in the screen object)

```json
[{
  // required
  "name": "List",
  "path": "server/assets/pages/tab-bar/index.html"
}],
```

Each object in pages array, extensions page object, consists of these fields:

- `name`: Required field, defines extension page's identity
- `path`: Required field, path to actual extension page implementation. Should be stored in `server` folder


#### themes

[Themes](/docs/coming-soon) represent files where you can provide set of styles for your UI components.

```json
[{
  // required
  "name": "Rubicon",

  // recommended
  "title": "Rubicon",
  "description": "Rubicon is a beautiful template built...",
  "showcase": ["server/assets/theme/rubicon.mp4","server/assets/theme/rubicon1.jpg", "server/assets/theme/rubicon2.jpg", "server/assets/theme/rubicon3.jpg"],

  // optional
  "icons": "app/themes/Rubicon/assets/icons/",
  "variablesSchema": "@.Rubicon"
}]
```

Each object in themes array, theme object, consists of these fields:

- `name`: Required field, defines theme's identity
- `title`: Theme's title
- `description:` Theme's description
- `showcase`: Array of strings which represent paths to multimedia files in `server` folder, such as videos and images, which present your theme. Dimensions for @2x quality resolution are 750 × 1334.
- `icons`: Path to icons of theme, should be stored in `app` asset's folder
- `variablesSchema`: Reference to a variable schema used by theme

#### variablesSchemas

Variable schemas are used to define the structure of the variables used by theme. These variables can be used to customize the theme.

```json
[{
  // required
  "name": "Rubicon",
  "path": "server/variables-schema/rubicon.json"
}]
```

Each object in variable schemas array, variable schema object, consists of these fields:

- `name`: Required field, defines variables schema name
- `path`: Required field, path to actual variables schema implementation. Should be stored in `server` folder


## Full example of extension.json

Finally, here's the full example of extension.json:

```json
{
  "shoutem": "1.0",
  "name": "restaurants",
  "version": "0.0.1",

  "title": "Restaurants",
  "website": "https://extensions.shoutem.com/shoutem.navigation",
  "description": "Make your users rate products.",
  "icon": "server/assets/extension/icon.png",
  "defaultLocale": "en",
  "settingsPages": [{
    "page": "@.settings",
    "title": "Settings",
  }],
  "settings": {
    "any-parameter": "any-value"
  },

  "shortcuts": [{
    "name": "RestaurantsList",
    "title": "Restaurants",
    "description": "Allow users...",
    "screen": "@.list",
    "iconUrl": "server/assets/extension/icon.png",
    "adminPages": [{
      "page": "@.CmsPage",
      "title": "Content",
      "parameters": {
        "schema": "@.Restaurants"
      },
    }],
    "settings": {
      "any-parameter": "any-value"
    }
  }],

  "screens": [{
    "name": "List",
    "title": "List",
    "image": "server/assets/screens/restaurants-list.png",
    "navigatesTo": [{
      "details": "@.Details"
    }]
  }, {
    "name": "Grid",
    "title": "Grid",
    "image": "server/assets/screens/restaurants-grid.png",
    "extends": "@.List",
  }, {
    "name": "Details",
    "title": "Details",
  }],

  "pages": [{
    "name": "settings",
    "path": "server/assets/settings/settings/index.html",
  }],

  "dataSchemas": [{
    "name": "Restaurants",
    "path": "server/data-schemas/restaurants.json"
  }],

  "themes": [{
    "name": "Rubicon",
    "title": "Rubicon",
    "variablesSchema": "@.Rubicon",
    "description": "Rubicon is a beautiful template built...",
    "showcase": ["server/assets/theme/rubicon.mp4","server/assets/theme/rubicon1.jpg", "server/assets/theme/rubicon2.jpg", "server/assets/theme/rubicon3.jpg"],
    "icons": "app/themes/Rubicon/assets/icons/"
  }, {
    "name": "Arno",
    "title": "Arno",
    "variablesSchema": "@.Rubicon",
    "description": "Arno is a beautiful template built...",
    "showcase": ["server/assets/theme/arno1.jpg", "server/assets/theme/arno2.jpg"],
    "icons": "app/themes/Arno/assets/icons/"
  }],

  "variablesSchemas": [{
    "name": "Rubicon",
    "path": "server/variables-schema/rubicon.json"
  }]
}
```
