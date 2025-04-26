# dev-quotes-homepage

Hello! :wave:

This is just a fun little plugin that I made to display some random coding/programming related quotes on the Backstage homepage. Although to be honest the card can be used anywhere you like! I hope you enjoy it!

If you want to leave a star, that would be awesome! :star:

I don't expect much work to be done on this plugin as it was just a fun little project I made. But if you would like to contribute, please do! See [Would you like to contribute? or add more quotes?](#would-you-like-to-contribute-or-add-more-quotes) for more information.

## Pre-requisites (if you want to use it on the homepage)

If you do want to use it on the homepage, first make sure you look over the official docs on how to configure the homepage [here](https://backstage.io/docs/getting-started/homepage/#composing-your-homepage). There is also the [homepage plugin](https://github.com/backstage/backstage/tree/master/plugins/home#readme) which might be better to read first.

## Installation

```bash
yarn add --cwd packages/app @parsifal-m/plugin-dev-quotes-homepage
```

Then all you need to do is import the card as below, anywhere you like!

```tsx
import { DevQuote } from '@parsifal-m/plugin-dev-quotes-homepage';

const overviewContent = (
  <Grid container spacing={3} alignItems="stretch">
    {entityWarningContent}
    <Grid item md={6}>
      <EntityAboutCard variant="gridItem" />
    </Grid>
    <Grid item md={6} xs={12}>
      <EntityCatalogGraphCard variant="gridItem" height={400} />
    </Grid>
    <Grid item md={4} xs={12}>
      <DevQuote />
    </Grid>

    <Grid item md={6} xs={12}>
      <OpaMetadataAnalysisCard />
    </Grid>

    <Grid item md={4} xs={12}>
      <EntityLinksCard />
    </Grid>
    <Grid item md={8} xs={12}>
      <EntityHasSubcomponentsCard variant="gridItem" />
    </Grid>
  </Grid>
);
```

## Example

An example quote

![Quote Image](static/img/quote.png)

An example quote on an entity page:

![Quote Entity Page](static/img/quote2.png)

## Would you like to contribute? or add more quotes?

Please do! I would love to see more quotes added to this plugin. If you would like to add some quotes, please open a pull request with your changes. If you would like to add a lot of quotes, please open an issue first so we can discuss it. And please keep them programming related, and keep it safe for work! Here is a short example of how to add a quote:

- Clone the repository and run `yarn install --immutable`
- Add the quote to the `quotes.ts` file in the `src` folder.
- Run `yarn prettier:write`
- Make sure to sign your commit with `git commit -s -m "Added quote by <someone>"`

## When will my quote be added to the plugin?

I will try and release a new version with the new quotes as soon as possible. Keep in mind this is a side project I made for fun!

## License

This project is released under the Apache 2.0 License.

## Export as Dynamic Plugin to be used on Red Hat Developer Hub

To deploy this plugin as a dynamic plugin on Red Hat Developer Hub, run the following commands:

1. Build locally:

```bash
yarn install
```

2. Export as dynamic plugin and tagged as a local container image

```bash
yarn build:image
```

Tag the version with the same version of the package:

```bash
podman tag localhost/plugin-dev-quotes-homepage-dynamic:latest localhost/plugin-dev-quotes-homepage-dynamic:3.0.7
```

3. Push into a container image, for example GitHub Container Registry:

```bash
podman push localhost/plugin-dev-quotes-homepage-dynamic:3.0.7 ghcr.io/rmarting/plugin-dev-quotes-homepage-dynamic:3.0.7
```

4. Define the dynamic plugin in the `rhdh-dynamic-plugins` ConfigMap as:

```yaml
      - package: oci://ghcr.io/rmarting/plugin-dev-quotes-homepage-dynamic:3.0.7!parsifal-m-plugin-dev-quotes-homepage
        disabled: false
        pluginConfig:
          dynamicPlugins:
            frontend:
              parsifal-m.plugin-dev-quotes-homepage:
                mountPoints:
                  - mountPoint: entity.page.overview/cards
                    importName: DevQuote
                    config:
                      layout:
                        gridColumnEnd:
                          lg: span 4
                          md: span 6
                          xs: span 12
                dynamicRoutes:
                  - importName: DevQuote
                    menuItem:
                      text: Quote
                    path: /devquote
```

Here, a full example of the `rhdh-dynamic-plugins` ConfigMap:

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: rhdh-dynamic-plugins
data:
  dynamic-plugins.yaml: |
    includes:
      - dynamic-plugins.default.yaml
    plugins:
      - package: oci://ghcr.io/rmarting/plugin-dev-quotes-homepage-dynamic:3.0.7!parsifal-m-plugin-dev-quotes-homepage
        disabled: false
        pluginConfig:
          dynamicPlugins:
            frontend:
              parsifal-m.plugin-dev-quotes-homepage:
                mountPoints:
                  - mountPoint: entity.page.overview/cards
                    importName: DevQuote
                    config:
                      layout:
                        gridColumnEnd:
                          lg: span 4
                          md: span 6
                          xs: span 12
                dynamicRoutes:
                  - importName: DevQuote
                    menuItem:
                      text: Quote
                    path: /devquote
```
