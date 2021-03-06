# gulp-clientlibify

> Integrate AEM with a styleguide

## Motivation
There is a push in the industry to code against an external style guide to maintain consistent styling and have a reusable
set of components to build applications with. In my experience working with AEM/CQ, integrating
a style guide into a project has consistently been a challenge. Each project generally ends up with a set of custom
scripts to achieve this integration (heavily scripted Grunt files, Groovy scripts, Git sub-trees, etc).

This plugin aims to take the guess work out of integrating AEM with an external style guide
by leveraging javascript tooling to build (and optionally deploy) a CRX package containing those front-end assets.

More info on Clientlibs on the [AEM Documentation](https://docs.adobe.com/docs/en/cq/5-6-1/developing/clientlibs.html).

This plugin is listed on the NPM registry [here](https://www.npmjs.com/package/gulp-clientlibify).

## Getting Started
Install this plugin with this command:

```shell
npm install gulp-clientlibify --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gulpfile with this line of JavaScript:

```js
var clientlibify = require('gulp-clientlibify');
```

## The "clientlibify" function

### Overview
In your project's Gulpfile, add a task and pipe your assets to the `clientlibify` function.

```js
gulp.task('clientlibify', function() {
		return gulp.src(assets/**/*)
				.pipe(clientlibify({
					//options go here
				}))
				.pipe(gulp.dest('dist'))
});
```

### Clientlib Options

#### options.cssDir
Type: `String`

A string value that is used to indicate where the `.css` or `.less` files should be fetched from.
This would likely be the path to your `css` directory. For example, `dist/css`.

#### options.jsDir
Type: `String`

A string value that is used to indicate where the `.js` files should be fetched from.
This would likely be the path to your `js` directory. For example, `dist/js`.

#### options.assetsDirs
Type: `Array`

An optional Array of paths to additional style guide assets that should be included in the CRX package.
This would likely be a set of paths to images, fonts, etc. For example, `dist/img`.
All directories specified will be copied over at the same level as the CSS and JS directories.
> This option only supports directories. Files will be ignored.

#### options.dest
Type: `String`
Default value: `tmp`

A string value that is used to indicate where the CRX package (.zip) should be placed.

#### options.installPackage
Type: `Boolean`
Default value: `false`

A boolean value that is used to indicate whether the generated CRX package should be deployed
and installed onto an AEM instance.
> If set to `true`, the `options.deploy*` options must be provided.

#### options.categories
Type: `Array`
Default value: `['etc-clientlibify']`

An array of string values that will be used as the clientlib categories.
> Note: the _first_ entry in this array will be used as the design folder name (i.e. /etc/designs/etc-clientlibify).

#### options.embed
Type: `Array`
Default value: `[]`

An array of string values that will be used as the clientlib embed values.

#### options.dependencies
Type: `Array`
Default value: `[]`

An array of string values that will be used as the clientlib dependencies.

### CRX Package Options

#### options.packageName
Type: `String`
Default value: `clientlibify`

A string value that is used as the CRX package name as well as part of the package
filename (name-version.zip).

#### options.packageVersion
Type: `String`
Default value: `1.0`

A string value that is used as the CRX package version as well as part of the package
filename (name-version.zip).

#### options.packageGroup
Type: `String`
Default value: `my_packages`

A string value that is used as the CRX package group (using AEM's default package group).

#### options.packageDescription
Type: `String`
Default value: `CRX package installed using the gulp-clientlibify plugin`

A string value that is used as the CRX package description.

### Deploy Options

#### options.deployScheme
Type: `String`
Default value: `http`

A string value that is used to define the scheme part of the URL (i.e. http, https, etc)

#### options.deployHost
Type: `String`
Default value: `localhost`

A string value that is used to define the host the CRX package will be deployed to.

#### options.deployPort
Type: `String`
Default value: `4502`

A string value that is used to define the port the CRX package will be deployed to.

#### options.deployUsername
Type: `String`
Default value: `admin`

A string value that is used to define the AEM username that will be used to authenticate
the package install HTTP request. This username will also be identified as the package installer.

#### options.deployPassword
Type: `String`
Default value: `admin`

A string value that is used to define the AEM password that will be used to authenticate
the package install HTTP request.

### Usage Examples

#### Default Options
In this example, only the mandatory configuration is provided (only one of `cssDir` or `jsDir` must be provided)
and the rest of the options use their default values. As a result, this task will create a CRX package
inside the `tmp` directory containing the `.css` and `.less` files under `assets/styles/css`. The file
will be name `clientlibify-1.0.zip` and is ready to be installed on an AEM instance.

```js
gulp.task('clientlibify', function() {
		return gulp.src(assets/**/*)
				.pipe(clientlibify({
					cssDir: 'assets/styles/css'
				}))
				.pipe(gulp.dest('dist'))
});
```

#### Custom Options
In this example, all options have been provided. As a result, a CRX package will be created inside
`dist` with the name `prickly-pear-2.1.zip`. This package will contain all the `.css`
and `.less` files under `assets/styles/css`, as well as the `.js` files under `assets/scripts`.
The important thing here is that the `installPackage` option has been set to `true`. This means that
once the CRX package is created, the task will install it on an AEM instance using the credentials
provided in the `options.deploy*` configuration.

```js
gulp.task('clientlibify', function() {
		return gulp.src(assets/**/*)
				.pipe(clientlibify({
		      dest: 'dist',
		      cssDir: 'assets/styles/css',
		      jsDir: 'assets/scripts',
		      assetsDirs: ['assets/images', 'assets/fonts'],
		      installPackage: true,
		      categories: ['awesome-styleguide'],
		      embed: [],
		      dependencies: ['cq-jquery'],

		      // package options
		      packageName: 'prickly-pear',
		      packageVersion: '2.1',
		      packageGroup: 'My Company',
		      packageDescription: 'CRX package installed using the gulp-clientlibify plugin',

		      // deploy options
		      deployScheme: 'http',
		      deployHost: 'localhost',
		      deployPort: '4502',
		      deployUsername: 'admin',
		      deployPassword: 'admin'
		    }))
				.pipe(gulp.dest('dist'))
});
```

Check out the [sample project](https://github.com/mickleroy/gulp-clientlibify/tree/master/sample-project) for more information.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality.

## Release History
n/a
