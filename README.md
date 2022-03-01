# Source maps uploader

This is a tiny build pack to run after the javascript assets are compiled, and source maps are generated to [upload it to Rollbar](https://docs.rollbar.com/docs/source-maps#3-upload-your-source-map-files), so we can have the source maps available before the first error occurrence.

## Usage

Add the build pack to your application:

```bash
heroku buildpacks:add volders/source-maps-uploader
```

After that, you need [activate the dyno metadata](https://devcenter.heroku.com/articles/dyno-metadata) for your app. During the build [we have](https://devcenter.heroku.com/articles/buildpack-api#bin-compile-summary) the `SOURCE_VERSION` env variable. However, we need the `HEROKU_SLUG_COMMIT` when the application is running to [set the `code_version` option](https://docs.rollbar.com/docs/source-maps#2-configure-the-rollbarjs-sdk-to-support-source-maps).

Next, add a `ROLLBAR_POST_SERVER_TOKEN` environment variable to your app with your `post_server_item` Rollbar token found in your project access tokens settings page found here: Settings → Project Access Tokens (`https://rollbar.com/volders/<the-project-name>/settings/access_tokens`.

Also, add an `APP_DOMAIN` environment variable with the application domain.

Then, add a `.source-maps-uploader` file in the root of your repository with the path to where the source maps are. For example:

```
public/packs/js
```

In the next deployment, you should see the uploaded files in the build log:

```
...
-----> Uploading source maps...
       Uploading file public/packs/js/application-ef490583af6aebb9bf03.js.map
       Uploading file public/packs/js/other-bc62c70ba43856d88229.js.map
       Source maps uploading completed
...
```

And on the Rollbar source maps page from the project: `https://rollbar.com/volders/<the-project-name>/settings/source_map`.

**Note**: This build pack does not work out of the box for the review apps because we can't activate the dyno metadata using the `app.json`. To use it in a review app, you must manually activate the dyno metadata and set the `APP_DOMAIN` to the correct review app address.

## License

Copyright 2022 Volders GmbH

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
