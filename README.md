# esbuild bundle with chalk v5 breaks due to "imports"

```sh
pnpm i

pnpm build
```

```
pnpm build

> esbuild --bundle index.ts

 > node_modules/.pnpm/chalk@5.0.0/node_modules/chalk/source/index.js:1:23: error: Could not resolve "#ansi-styles" (mark it as external to exclude it from the bundle)
     1 │ import ansiStyles from '#ansi-styles';
       ╵                        ~~~~~~~~~~~~~~
   node_modules/.pnpm/chalk@5.0.0/node_modules/chalk/package.json:11:18: note: The module "./source/vendor/ansi-styles/index.js" was not found on the file system
    11 │     "#ansi-styles": "./source/vendor/ansi-styles/index.js",
       ╵                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 > node_modules/.pnpm/chalk@5.0.0/node_modules/chalk/source/index.js:2:26: error: Could not resolve "#supports-color" (mark it as external to exclude it from the bundle)
     2 │ import supportsColor from '#supports-color';
       ╵                           ~~~~~~~~~~~~~~~~~
   node_modules/.pnpm/chalk@5.0.0/node_modules/chalk/package.json:14:14: note: The module "./source/vendor/supports-color/browser.js" was not found on the file system
    14 │       "default": "./source/vendor/supports-color/browser.js"
       ╵                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2 errors
node:child_process:867
    throw err;
```

Chalk v5 package.json:

```json
{
  "name": "chalk",
  "version": "5.0.0",
  "description": "Terminal string styling done right",
  "license": "MIT",
  "repository": "chalk/chalk",
  "funding": "https://github.com/chalk/chalk?sponsor=1",
  "type": "module",
  "exports": "./source/index.js",
  "imports": {
    "#ansi-styles": "./source/vendor/ansi-styles/index.js",
    "#supports-color": {
      "node": "./source/vendor/supports-color/index.js",
      "default": "./source/vendor/supports-color/browser.js"
    }
  }
  //...
}
```
