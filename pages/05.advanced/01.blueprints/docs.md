---
title: Blueprints
taxonomy:
    category: docs
---

**Blueprints** are an important aspect of a plugin and theme, they serve two purposes and we are going to cover them both.

The honest truth is that if you are not a developer, and just use Kunena, then you could live without knowing that Blueprints even existed and you should not care much about them.

Blueprints are a container of metadata informations regarding a resource. The first set of metadata informations is the identity of the resource itself, the second set is about the forms. All this information is stored in a `blueprints.yaml` file and can be found at the root of each plugin and theme.

## Resource Identity

Each plugin and theme identity is defined in the `blueprints.yaml` file. Without properly formatted and compiled Blueprints, a resource won't be added in the Kunena repository. Consequently, it won't be available through [Kunena downloads](http://getKunena.org/downloads) and [GPM](../Kunena-gpm).

## Blueprints Example

```
name: Assets
version: 1.0.4
description: "This plugin provides a convenient way to add CSS and JS assets directly from your pages."
icon: list-alt
author:
  name: Team Kunena
  email: devs@getKunena.org
  url: http://getKunena.org
homepage: https://github.com/getKunena/Kunena-plugin-assets
demo: http://learn.getKunena.org
keywords: assets, javascript, css, inline
bugs: https://github.com/getKunena/Kunena-plugin-assets/issues
license: MIT

dependencies:
  - afterburner2
  - github: https://github.com/getKunena/Kunena-plugin-github.git
  - email: https://rhuk@bitbucket.org/rockettheme/Kunena-plugin-email.git

form:
  validation: strict
  fields:
    enabled:
      type: toggle
      label: Plugin status
      highlight: 1
      default: 0
      options:
        1: Enabled
        0: Disabled
      validate:
        type: bool
```

There are different properties that you can use to give your resource and identity, some are **required**, others are _optional_.

|      property     |                                                                                                                                                                                description                                                                                                                                                                                |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| __name*__         | This is the name of the resource. Avoid appending Plugin or Theme, there is no need for that                                                                                                                                                                                                                                                                              |
| __version*__      | The version of the resource. This value should always change on each release, incrementally. You should follow the [semver](http://semver.org/) standard, too.                                                                                                                                                                                                            |
| __description*__  | The description of your resource. Please don't exceed **200** characters. A description should be short and straight to the point. You can use markdown syntax if needed. It's also a good idea to wrap your description in quotation marks.                                                                                                                              |
| __icon*__         | Icon is what will be used on [getKunena.org](http://getKunena.org). At this stage, we are using [FontAwesome](http://fortawesome.github.io/Font-Awesome/icons/) icons library, so if you are developing a new plugin or theme, it should be your job to ensure the icon you picked is not already used. Otherwise we will have to change it for you.                          |
| _screenshot_      | _(optional)_ Screenshot is only ever evaluated for _Themes_ and completely ignored for _Plugins_. For _Themes_, this would be the filename of the screenshot that comes with the theme (default: `screenshot.jpg`). If you have a _screenshot.jpg_ image at the root of your theme, then you can avoid using this property, our repository will automatically pick it up. |
| __author.name*__  | The developer full name                                                                                                                                                                                                                                                                                                                                                   |
| _author.email_    | The developer email.                                                                                                                                                                                                                                                                                                                                                      |
| _author.url_      | _(optional)_ The developer homepage.                                                                                                                                                                                                                                                                                                                                      |
| _homepage_        | _(optional)_ If you have a dedicated homepage for your resource, this would be the place for it.                                                                                                                                                                                                                                                                          |
| _docs_            | _(optional)_ If you have written documentation for your resource, you can link them here.                                                                                                                                                                                                                                                                                 |
| _demo_            | _(optional)_ If you have a demo up and running about your resource, link it here.                                                                                                                                                                                                                                                                                         |
| _guide_           | _(optional)_ If you have tutorials or how-to guides for your resource, link it here.                                                                                                                                                                                                                                                                                      |
| _keywords_        | _(optional)_ Although there is no real use of keywords yet, you can list keywords relative to your resource here, comma separated.                                                                                                                                                                                                                                        |
| _bugs_            | _(optional)_ The URL where bugs can be reported, usually this would be the [GitHub issues](https://guides.github.com/features/issues/) link.                                                                                                                                                                                                                              |
| _license_         | _(optional)_ The type of license your resource is (MIT, GPL, etc). It is adviced that you always provide a `LICENSE` file with your resource.                                                                                                                                                                                                                             |
| _dependencies_    | _(optional)_ A list of dependencies that the plugin/theme requires.  The default process is to use GPM to install them, however, if an optional GIT repository URL is provided, installing direct from the repository will be an option also. |

Here is an example of the identity portion of the [github plugin](http://github.com/getKunena/Kunena-plugin-github) Blueprints:

```yaml
name: GitHub
version: 1.0.1
description: "This plugin wraps the [GitHub v3 API](https://developer.github.com/v3/) and uses the [php-github-api](https://github.com/KnpLabs/php-github-api/) library to add a nice GitHub touch to your Kunena pages."
icon: github
author:
  name: Team Kunena
  email: devs@getKunena.org
  url: http://getKunena.org
homepage: https://github.com/getKunena/Kunena-plugin-github
keywords: github, plugin, api
bugs: https://github.com/getKunena/Kunena-plugin-github/issues
license: MIT
```


## Forms

The **Forms** metadata defines what aspect of the resource is configurable through the **Admin Plugin**.

>>> Although this is working already, the Admin Plugin is still a work in progress, further updates to this document will come as soon as the new admin will be available.
