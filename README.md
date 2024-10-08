# Elections 2024 precinct map generator

This is a template for building Svelte apps at the Star Tribune. It's intended to replace the old svelte+webpack template.

## Dataviz generation

Instructions for reporters, graphic designers and anyone else:

1. [Launch the map configuration page](https://striblab.github.io/elex24_config/dist)

2. Select which electoral results to display and map configuration options

3. Click GENERATE and an embeddable Mapbox URL will pop into a new browser tab

4. Download necessary GEOJSON shapefiles and result CSVs for desired contests and geographies from the list below the configuration options.


## Starting a project

To start a new project based on this template, run

```
npx degit striblab/strib-svelte-vite-template#main name-of-your-new-project
cd name-of-your-new-project
npm install
```

## Development

To run the dev server run `npm run dev`.

To build for deployment, run `npm run build`.

## Strib webfonts

The licenses for webfonts used by the Star Tribune stipulate the fonts can only be served from startribune.com
URLs, meaning they are blocked from localhost while you are developing. To get around this, run the shell script `npm run get-fonts` to download .woff font files directly to the `/public/strib-webfonts/` directory.
The local webfonts themselves and the webfont SCSS file that references them are excluded from build and deploy processes. If deployed to static.startribune.com, Star Tribune webfonts will render as they normally should without the need for local font files.


## Deploying

Deployment is currently handled by a shell script (`strib-deploy.sh`). You need to have the [AWS CLI](https://aws.amazon.com/cli/) installed
with credentials for the static.startribune.com bucket using the 'default' profile.

To deploy to staging, run `npm run deploy-staging`. For production, run `npm run deploy-production`. This will upload to a typical path using your project's name, which is read from the root directory of your project (where `package.json` is located). 