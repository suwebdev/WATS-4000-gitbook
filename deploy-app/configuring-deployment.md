# Configuring Deployment

To configure the deployment of our Vue.js application, we will leverage the ability of Github Pages to serve a website from the `docs` folder in a project repository. This means that we need to alter our configuration so that Webpack builds the code into the `docs/` directory instead of the `dist/` directory \(which is the default location Webpack uses\).

**vue.config.js**

To make this happen we will need to add a `vue.config.js` file that provides instructions for building to the `docs` directory.  Create this file in the root of your project.  The designation of `publicPath: ''` and `outputDir: './docs/'` will allow us to us deploy to a path under an account on `github.io` .  Setting `assetsDir: 'assets'` means that out `css` and `js` folder will be built under the `./docs/assets` directory.

```
const path = require('path');
module.exports = {
    configureWebpack: {
        resolve: {
            //allow for @ or @src alias for src
            alias: require('./aliases.config').webpack
        }
    },
    chainWebpack: config => {
        //turn off elint for webpack transpile
        config.module.rules.delete('eslint');
    },
    runtimeCompiler: true,
    css: {
        sourceMap: true
    },
    publicPath: '',
    //build for docs folder to enable gh-pages hosting
    outputDir: './docs/',
    assetsDir: 'assets'
}
```

**aliases.config.js**

The reference to the `./aliases.config.js` file means we need to add the file to our root as well.  This file provides the convenience of references files that are stored in the `src` directory with the `@` symbol.  For example following imports are equivalent:

```
import HelloWorld from '@/components/HelloWorld.vue'
import HelloWorld from 'src/components/HelloWorld.vue'
```

Create the `./aliases.config.js` file and paste the following instructions into it.

```
const path = require('path')
function resolveSrc(_path) {
  return path.join(__dirname, _path)
}
const aliases = {
  '@': 'src',
  '@src': 'src'
}
module.exports = {
  webpack: {},
  jest: {}
}
for (const alias in aliases) {
  module.exports.webpack[alias] = resolveSrc(aliases[alias])
  module.exports.jest['^' + alias + '/(.*)$'] =
    '<rootDir>/' + aliases[alias] + '/$1'
}

```





Once we have made those changes to the `config/index.js` file, we can run the commands to build and deploy the site.

