# Testing metalsmith-layouts

[![Greenkeeper badge](https://badges.greenkeeper.io/tschaub/test-metalsmith-layouts.svg)](https://greenkeeper.io/)

This repo includes a test of `metalsmith-layouts` with a partial.

The `layouts/nested/layout.html` template refers to a partial named `partial`.  The `metalsmith.json` config includes `"partials": "partials"`.  My assumption is that the `partials` directory should be relative to the current working directory.

Running `npm test` shows this failure:

```
$ npm test

  Metalsmith · ENOENT: no such file or directory, open '/Users/tschaub/projects/test-metalsmith-layouts/layouts/partials/partial.html'

Error: ENOENT: no such file or directory, open '/Users/tschaub/projects/test-metalsmith-layouts/layouts/partials/partial.html'
    at Error (native)
```

This makes it look like the `partials` directory is supposed to be relative to the `layouts` directory (instead of the current working directory).

If you move the `partials` directory to `layouts/partials`, you get a different failure.

```
$ mv partials/ layouts/
$ npm test

  Metalsmith · The partial partial could not be found

Error: The partial partial could not be found
    at Object.invokePartial (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/runtime.js:266:11)
    at Object.invokePartialWrapper [as invokePartial] (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/runtime.js:68:39)
    at Object.eval (eval at createFunctionContext (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/compiler/javascript-compiler.js:254:23), <anonymous>:10:28)
    at main (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/runtime.js:173:32)
    at ret (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/runtime.js:176:12)
    at ret (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/handlebars/dist/cjs/handlebars/compiler/compiler.js:525:21)
    at /Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:734:16
    at /Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:143:5
    at Promise._execute (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/bluebird/js/release/debuggability.js:272:9)
    at Promise._resolveFromExecutor (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/bluebird/js/release/promise.js:468:18)
    at new Promise (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/bluebird/js/release/promise.js:77:14)
    at promisify (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:136:10)
    at Function.exports.handlebars.render (/Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:724:10)
    at /Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:164:27
    at /Users/tschaub/projects/test-metalsmith-layouts/node_modules/consolidate/lib/consolidate.js:98:5
    at FSReqWrap.readFileAfterClose [as oncomplete] (fs.js:380:3)
```

One way to get things working is to have both `partials/partial.html` and `layouts/partials/partial.html`.  And the `partials/partial.html` can be an empty file.

```
$ mkdir partials && touch partials/partial.html
$ npm test

  Metalsmith · successfully built to /Users/tschaub/projects/test-metalsmith-layouts/build
```

I'm using `node@4.2.4` and `npm@3.5.2` with other package versions listed in `package.json`.
