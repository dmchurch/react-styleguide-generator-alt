# 3.17.0 (2017-04-30)
  * Sort indexes numerically rather than lexicologically
  * Added an exampleOpts prop with firstTabName to configure the name of the first example tab

# 3.16.0 (2017-01-18)
  * Allow configuration of webpack dev server via devServer stanza of webpack config.

# 3.15.1 (2017-01-18)
  * Reload page when module cannot hot reload
  
# 3.15.0 (2016-08-25)

  * Replace the custom `deepmerge` package via git with `deepmerge-alt`

# 3.14.0 (2016-08-22)
  * scope json loader to appDir

# 3.13.1 (2016-08-17)
  * Updated tabs to use NPM module `react-simpletabs-alt` instead of `github:matthewgertner/react-simpletabs`
    * No change in functionality
    * React 15.x compatible
  * Upgraded `react-bootstrap` dependency minor version
    * React 15.x compatible

# 3.13.0 (2016-08-08)
* Add baseStyle boolean option to disable inclusion of base stylesheet
  * -b or --baseStyle on CLI
  * "baseStyle" property in configuration JSON

# 3.12.4 (2016-07-31)
  * Allow "entry" value of webpack configuration to be either Object or Array

# 3.12.3 (2016-07-19)
  * Fix broken demo issue with optional render for tabbed examples
  * Provide examples of stateless components and createClass static usage

# 3.12.1 (2016-07-02)
  * Fixed wrong path to 'publicPath' variable from webpackConfig

# 3.11.2 (2016-07-01)
  * Fix to render also pure function components

# 3.11.1 (2016-06-22)
  * Fix coding style violations
  * Fixing all npm modules to specific versions

# 3.11.0 (2016-06-14)
  * Allow additional Babel plugins to be configured

# 3.10.0 (2016-06-07)
  * Fixes category navigation when using root or pushstate.
  * Make bundle paths absolute to fix landing on subpaths when using pushstate

# 3.9.0 (2016-06-02)
  * Allow indices of 10 and above by supporting floats.

# 3.8.1 (2016-05-27)
  * Resolves conflict between pushstate and relative navigation paths

# 3.8.0 (2016-05-25)
  * Fixes issue where overridden publicPath would break dev mode

# 3.7.0 (2016-05-23)
  * Preventing .babelrc conflicts with RSG compilation

# 3.6.0 (2016-05-16)
  * Expanding backwards compatibility to include node@0.x

# 3.5.3 (2016-05-09)
  * Adding json-loader to webpack loaders to handle json requires

# 3.5.1 (2016-03-30)
  * Fixes "show all" link issue when serving from sub-directory

# 3.5.0 (2016-02-21)
  * Add feature to give example components initial `props`

# 3.4.0 (2016-02-16)
  * Bumping previous commit to 3.4.0 instead of 3.3.1

# 3.3.1 (2016-02-16)
  * Removes 'vendor' feature from webpack config to allow npm2 compatibility.

# 3.3.0 (2016-02-15)

  * Bumps Babel to 6.5.2. Fixes #9. No more unnecessary semicolons enforcement.

# 3.2.2 (2016-02-05)

  * Filters components without `styleguide` field. Fixes #16: Fails if input files have no styleguide field

# 3.2.1 (2016-01-28)

  * patch version bump so npm shows the latest true version

# 3.2.0 (2016-01-28)

  * Fix issues with babel 6 found in
    * https://github.com/theogravity/react-styleguide-generator-alt/issues/12
    * https://github.com/theogravity/react-styleguide-generator-alt/issues/11

# 3.1.1 (2016-01-11)

  * Change hmr server hostname to 0.0.0.0 so we can use with vagrant/virtualbox

# 2.6.0, 3.1.0 (2016-01-07)

  * Allow passing a Javascript file for the `--config` option. This will allow usage of `webpackConfig` outside of using `gulp` or `grunt`
  * Note that babel 6.4 requires semicolons in your class properties https://github.com/feross/standard/issues/372; packages have been fixed to 6.3 for now due to conflicts with using `standard.

# 2.5.4, 3.0.2 (2015-12-13)

  * Fix "show all" link where it was not using the configured base (if used)

# 2.5.2, 3.0.1 (2015-12-13)

  * Fix syntax highlighting when individual components in the sidebar is selected
  * Adjust component category spacing
  * Add show all link on sidebar

# 3.0.0 (2015-12-13)

  * Support for babel 6. Use the 2.x versions for babel 5 support.

# 2.5.1 (2015-12-13)

  * Show components in each category in the sidebar

# 2.4.0 (2015-12-11)

  * Base exclusion `babel-loader` rule is no longer `path.resolve(process.cwd(), 'node_modules')`, but instead `node_modules`. This is mainly for projects that need to compile guides against directories that contain individually packaged components that have their own `node_modules` directory.

# 2.3.0 (2015-12-08)

  Performance enhancements:

  * separate vendor components into another bundle
  * Cache highlighted nodes so they are not re-processed. For really large pages, the highlighter can take a LOT of time for processing before the page is re-rendered. This fix hopefully resolves most of that.
  * `highlight.js` updated to `9.x`

# 2.2.0 (2015-12-01)

  * Add `webpackConfig` configuration file option to extend the rsg webpack configuration

# 2.1.1 (2015-12-01)

  * Fix issue where the props metadata file was not being written out if no propdocs were defined

# 2.1.0 (2015-11-30)

  * add `transpileIncludes` option to the config to allow for additional rules for babel transpiling

# 2.0.5 (2015-11-24)

  * Fix major bug as a result of renaming the package from fork

# 2.0.0 (2015-11-24)

  * First alt version, forked from `react-styleguide-generator`
  * Removed `browserify` and replaced with `webpack` + hot module replacement
  * Complete overhaul of the core `rsg.js` lib to support `webpack`
  * `react-docgen` generation and asset distribution moved to custom webpack plugins
  * Fixed a bug where using input text boxes and typing into them will shift focus to the search box
