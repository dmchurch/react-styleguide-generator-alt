**1/13/2018 - This project is now in maintenance mode. No new features will be accepted. I recommend the [react-storybook](https://storybook.js.org/) project instead.**

# React Styleguide Generator (Alt!)

[![npm](https://img.shields.io/npm/v/rsg-alt.svg)](https://npmjs.org/package/rsg-alt)

* Use the `2.x` versions for babel 5, `3.x` for babel 6.'
* PRs are welcome, feel free to contribute to the project!

A React component guide that generates code examples, prop documentation, rendered component samples and is built for active development.

* Renders out components that can be interacted with
* Uses `webpack` + hot module replacement to automatically update the guide as you work on your components
* `react-docgen` is used under the hood to document your component's `propTypes`
* Can manually + automatically generate code samples w/ syntax highlighting

![preview](https://cloud.githubusercontent.com/assets/855434/11770687/a754aea4-a1b9-11e5-8ae2-1b5db451c8d5.png)  
[Demo](http://theogravity.github.io/react-styleguide-generator-alt/) using the [React-Bootstrap](http://react-bootstrap.github.io/).

## Fork notice

This project is originally forked from [react-styleguide-generator](https://github.com/pocotan001/react-styleguide-generator).

Differences:

* Removed `browserify` and replaced with `webpack` + hot module replacement
* Complete overhaul of the core `rsg.js` lib to support `webpack`
* `react-docgen` generation and asset distribution moved to custom webpack plugins
* Fixed a bug where using input text boxes and typing into them will shift focus to the search box
* Improved highlighting performance - extremely large guides should not take forever to render
* Config file can now be an exported object, allowing for more dynamic configuration

See `HISTORY.md` for future update info

## Prereqs

* React 0.14.x. Install both `react` and `react-dom`.
* Was developed on node 4, but has been known to work on ~0.10.45.

## Installation

``` sh
npm install rsg-alt
```

### Documenting your React components

Create file for the styleguide, and then add some documentation to a static field named `styleguide`. You can use the [ES6 syntax](https://github.com/lukehoban/es6features) by [Babel](https://babeljs.io/).

``` js
import React from 'react'
import Button from './Button'

export default class extends React.Component {
  static styleguide = {
    index: '1.1',
    category: 'Elements',
    title: 'Button',
    description: 'You can use **Markdown** within this `description` field.',
    code: `<Button size='small|large' onClick={Function}>Cool Button</Button>`,
    className: 'apply the css class',
    props: {
      size: 'large'
    }
  }

  onClick () {
    alert('Alo!')
  }

  render () {
    return (
      <Button size='large' onClick={this.onClick}>Cool Button</Button>
    )
  }
}
```

- `index`: Reference to the element's position in the styleguide (optional)
- `category`: Components category name
- `title`: Components title
- `description`: Components description (optional)
- `code`: Code example (optional). Not specifying this will not auto-generate an example.
- `className`: CSS class name (optional)
- `props`: Properties to assign to the rendered example component (optional)

#### Additional examples in tabs (optional) [Demo](http://theogravity.github.io/react-styleguide-generator-alt/#!/Features!/Additional%20examples%20in%20tabs)

You can optionally use tabs to segment out examples for a component:

``` js
import React from 'react'
import Button from './Button'

export default class extends React.Component {
  static styleguide = {
    …
    // Component to use for generating additional examples
    exampleComponent: Button,
    // Options for the first tab
    defaultTabOpts: {
        firstTabName: 'Default example'
    },
    // Array of additional example tabs
    examples: [{
      tabTitle: 'Default',
      props: {
        children: 'Default'
      }
    }, {
      tabTitle: 'Primary',
      props: {
        kind: 'primary',
        children: 'Primary',
        onClick () {
          alert('o hay!')
        }
      }
    }]
  }
}
```

- `exampleComponent`: `ReactElement` to use to generate the examples.
- `examples`: Array of examples, which generates additional tabs of example components and sample code
- `examples[].tabTitle`: Title of example tab
- `examples[].props`: Properties to assign to the rendered example component
- `examples[].props.children`: (optional) Child elements to assign to the example component
- `examples[].code`: (optional) Code example. Omitting this will attempt to auto-generate a code example using the `examples[].props`

#### Additional examples via doc comment (optional) [Demo](http://theogravity.github.io/react-styleguide-generator-alt/#!/Features!/Additional%20examples%20via%20doc%20comment)

Doc comment support example is:

``` js
/**
 * Substitute this description for `styleguide.description`.
 */
export default class extends Component {
  // required for prop documentation
  static displayName = 'ExampleButton'

  static styleguide = {
    …
  }

  // Document the props via react-docgen
  static propTypes = {
    /**
     * Block level
     */
    block: React.PropTypes.bool,
    /**
     * Style types
     */
    kind: React.PropTypes.oneOf(['default', 'primary', 'success', 'info'])
  }

  render () {
    return <Button block kind='primary'>Cool Button</Button>
  }
}
```

If necessary, visit [react-styleguide-generator-alt/example](https://github.com/theogravity/react-styleguide-generator-alt/tree/master/example) to see more complete examples for the documenting syntax.

### Generating the documentation

#### Command line tool

A common usage example is below.

``` sh
# The default output to `styleguide` directory
rsg 'example/**/*.js'
```

Type `rsg -h` or `rsg --help` to get all the available options.

```
Usage: rsg [input] [options]

Options:
  -o, --output     Output directory            ['styleguide']
  -t, --title      Used as a page title        ['Style Guide']
  -r, --root       Set the root path           ['.']
  -f, --files      Inject references to files  ['']
  -c, --config     Use a js/json config file   ['styleguide.json']
  -p, --pushstate  Enable HTML5 pushState      [false]
  -v, --verbose    Verbose output              [false]
  -d, --dev        Start server with webpack hmr [3000]

Examples:
  rsg 'example/**/*.js' -t 'Great Style Guide' -f 'a.css, a.js' -v

  # Necessary to use a config file if you want to enable react-docgen
  rsg 'example/**/*.js' -c 'styleguide.json' -v

  # Example 2 - config file does module.exports = { ... }
  rsg 'example/**/*.js' -c 'styleguide.js' -v
```

#### Gulp

``` js
var gulp = require('gulp')
var RSG = require('rsg-alt')

gulp.task('styleguide', function (done) {
  var rsg = RSG('example/**/*.js', {
    output: 'path/to/dir',
    files: ['a.css', 'a.js']
  })

  rsg.generate(function (err) {
    if (err) {
      console.error(String(err))
    }

    done()
  })
})
```

#### Grunt

``` js
var RSG = require('rsg-alt')

grunt.registerTask('rsg', 'React style guide', function () {
  var done = this.async()

  try {
    var conf = grunt.config.get('rsg')

    RSG(conf.input, {
      config: conf.configFile,
      dev: false,
      verbose: true
    }).generate(function (err) {
      if (err) {
          grunt.log.error('Error: ' + err + ' ' + err.stack())
          return done(false)
      }

      grunt.log.ok('react styleguide generation complete')
      done()
    })
  } catch (e) {
    grunt.log.error('Error: ' + e + ' ' + e.stack)
    done(false)
  }
})
```

## API

### RSG(input, [options])

Returns a new RSG instance.

#### input

Type: `String`

Refers to [glob syntax](https://github.com/isaacs/node-glob) or it can be a direct file path.

#### options

##### output

Type: `String`  
Default: `'styleguide'`

Output directory path.

##### title

Type: `String`  
Default: `'Style Guide'`

Used as a page title and in the page header.

##### reactDocgen.files

Type: `Array`
Default: `input`

An array of `glob`-able file/paths for `react-docgen` to parse. If not specified, will default the value to `input`.

##### root

Type: `String`  
Default: `'.'`

Set the root path. For example, if the styleguide is hosted at `http://example.com/styleguide` the `options.root` should be `styleguide`.

##### files

Type: `Array`  
Default: `null`

Inject references to files. A usage example is:

``` js
{
  files: [
    '//maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css',
    'a.css',
    'a.js',
    'icon.svg'
  ]
}
```

Check for the existence of the files and only copy the files if it exists.

```
styleguide/files
├─ a.css
├─ a.js
└─ icon.svg
```

Inject file references into index.html if the files with the extension `.css` or `.js`.

``` html
<!doctype html>
<html>
  <head>
    …
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css">
    <link rel="stylesheet" href="files/a.css">
  </head>
  <body>
    …
    <script src="files/a.js"></script>
  </body>
</html>
```

##### config

Type: `String|Object`  
Default: `styleguide.json`

The entire range of RSG API options is allowed. [Usage example](https://github.com/theogravity/react-styleguide-generator-alt/blob/master/example/styleguide.json).

- An object can be passed instead of a filename that contains the RSG API options.
- A Javascript file can be passed in that exports an object instead:

```js
// styleguide.js
module.exports = {
  "title": "React Style Guide",
  "files": [
    "//maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css",
    "example/example.css"
  ],
  "webpackConfig": {}
}
```

##### pushstate

Type: `String`  
Default: `false`

Enable HTML5 pushState. When this option is enabled, styleguide will use history API.

##### webpackConfig

Type: `Object`
Default: `{}`

Uses `deepmerge` to merge in a custom webpack configuation to the rsg webpack configuration. Existing arrays (eg plugins) are appended to maintain functionality.

##### transpileIncludes

Type: `Array<String|RegExp>`
Default: `null`

Adds a custom rule(s) to the webpack loader to include additional items to transpile via `babel-loader`. This is provided as a convenience to using `webpackConfig` directly.

#### babelPlugins

Type: `Array<String>`
Default: `null`

Add Babel plugins in addition to those automatically provided with the presets `react`, `es2015`, `stage-0`. EG: You might want to support decorators with `transform-decorators-legacy`.

### rsg.generate([callback])

Generate the files and their dependencies into a styleguide output.

## Demo

Get the demo running locally:

``` sh
git clone git@github.com:theogravity/react-styleguide-generator-alt.git
cd react-styleguide-generator-alt/example/
npm install
npm start
```

Visit [http://localhost:3000/](http://localhost:3000/) in your browser.

## Remove the `styleguide` static for production

It is most likely that you will not have use for the `styleguide` static when you're using your component in production. To remove the static (and other unnecessary statics) during transpiling, use [babel-plugin-transform-react-remove-statics](https://www.npmjs.com/package/babel-plugin-transform-react-remove-statics)

## Troubleshooting

### Error: No suitable component definition found.

Make sure your component contains `displayName` and `render()`.
