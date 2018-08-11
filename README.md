[![Build Status](https://travis-ci.org/Microsoft/TypeScript.svg?branch=master)](https://travis-ci.org/Microsoft/TypeScript)
[![VSTS Build Status](https://typescript.visualstudio.com/_apis/public/build/definitions/cf7ac146-d525-443c-b23c-0d58337efebc/4/badge)](https://typescript.visualstudio.com/TypeScript/_build/latest?definitionId=4&view=logs) 
[![npm version](https://badge.fury.io/js/typescript.svg)](https://www.npmjs.com/package/typescript)
[![Downloads](https://img.shields.io/npm/dm/typescript.svg)](https://www.npmjs.com/package/typescript)

# TypeScript

**Patched version of TypeScript to support MJS file emission.**

Install the `ts-mjs` package as a `devDependency` with `yarn add -D ts-mjs` or `npm install --save-dev ts-mjs`.

Everything stays the same, but you can now call the patched version of `tsc` as `tsc-mjs` and if you can add the compiler option `--mjs` to emit `.mjs` files instead of `.js` files.

These are required for NodeJS to correctly identify MJS modules as such.

If you use the `--sourceMap` compiler option, this will also emit the corresponding `.mjs.map` source map for your `.mjs` file.

If the output target for JSX would be preserve via one of the compiler options, regular JSX code with the regular extension `.jsx` will be emitted.

For example, if you run

```sh
tsc --sourceMap --declaration --target es3 --module commonjs index.ts
```

this will emit

```txt
index.d.ts (the declaration file)
index.js (the CommonJS ES3 module)
index.js.map (the source map for the CommonJS module)
```

If you run

```sh
tsc-mjs --sourceMap --declaration --target esnext --module esnext --mjs index.ts
```

```txt
index.d.ts (the declaration file)
index.mjs (the ES module)
index.mjs.map (the source map for the ES module)
```

The declaration files that are generated will be identical in both cases.

You can use this to publish hybrid modules by running both commands in succession (or in parallel, since they won't interfere with each other). The output will be:

```txt
index.d.ts (the declaration file)
index.js (the CommonJS ES3 module)
index.js.map (the source map for the CommonJS module)
index.mjs (the ES module)
index.mjs.map (the source map for the ES module)
```

If you have other source files, the corresponding 5 files will be generated for them.

If someone enters your package now via an `.mjs` file, node will automatically search for `.mjs` file in all `import` (or `require`) statements (and it will fall back to `.js` should an `.mjs` file not exist).

In your `package.json`, you can make the `main` field "dynamic" by removing the extension of the linked file.

```json
{
    "main": "index",
    "browser": "index.js",
    "module": "index.mjs",
    "types": "index.d.ts"
}
```

For a real-life example of this being in use, check out my `hsluv-ts` library ([Github](https://github.com/gfmio/hsluv-ts), [NPM](https://www.npmjs.com/package/hsluv-ts)).

---

[![Join the chat at https://gitter.im/Microsoft/TypeScript](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Microsoft/TypeScript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[TypeScript](https://www.typescriptlang.org/) is a language for application-scale JavaScript. TypeScript adds optional types to JavaScript that support tools for large-scale JavaScript applications for any browser, for any host, on any OS. TypeScript compiles to readable, standards-based JavaScript. Try it out at the [playground](https://www.typescriptlang.org/play/), and stay up to date via [our blog](https://blogs.msdn.microsoft.com/typescript) and [Twitter account](https://twitter.com/typescriptlang).

## Installing

For the latest stable version:

```bash
npm install -g typescript
```

For our nightly builds:

```bash
npm install -g typescript@next
```

## Contribute

There are many ways to [contribute](https://github.com/Microsoft/TypeScript/blob/master/CONTRIBUTING.md) to TypeScript.
* [Submit bugs](https://github.com/Microsoft/TypeScript/issues) and help us verify fixes as they are checked in.
* Review the [source code changes](https://github.com/Microsoft/TypeScript/pulls).
* Engage with other TypeScript users and developers on [StackOverflow](https://stackoverflow.com/questions/tagged/typescript). 
* Join the [#typescript](https://twitter.com/#!/search/realtime/%23typescript) discussion on Twitter.
* [Contribute bug fixes](https://github.com/Microsoft/TypeScript/blob/master/CONTRIBUTING.md).
* Read the language specification ([docx](https://github.com/Microsoft/TypeScript/blob/master/doc/TypeScript%20Language%20Specification.docx?raw=true),
 [pdf](https://github.com/Microsoft/TypeScript/blob/master/doc/TypeScript%20Language%20Specification.pdf?raw=true), [md](https://github.com/Microsoft/TypeScript/blob/master/doc/spec.md)).

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see 
the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) 
with any additional questions or comments.

## Documentation

*  [Quick tutorial](https://www.typescriptlang.org/docs/tutorial.html)
*  [Programming handbook](https://www.typescriptlang.org/docs/handbook/basic-types.html)
*  [Language specification](https://github.com/Microsoft/TypeScript/blob/master/doc/spec.md)
*  [Homepage](https://www.typescriptlang.org/)

## Building

In order to build the TypeScript compiler, ensure that you have [Git](https://git-scm.com/downloads) and [Node.js](https://nodejs.org/) installed.

Clone a copy of the repo:

```bash
git clone https://github.com/Microsoft/TypeScript.git
```

Change to the TypeScript directory:

```bash
cd TypeScript
```

Install Jake tools and dev dependencies:

```bash
npm install -g jake
npm install
```

Use one of the following to build and test:

```
jake local            # Build the compiler into built/local 
jake clean            # Delete the built compiler 
jake LKG              # Replace the last known good with the built one.
                      # Bootstrapping step to be executed when the built compiler reaches a stable state.
jake tests            # Build the test infrastructure using the built compiler. 
jake runtests         # Run tests using the built compiler and test infrastructure. 
                      # You can override the host or specify a test for this command. 
                      # Use host=<hostName> or tests=<testPath>. 
jake runtests-browser # Runs the tests using the built run.js file. Syntax is jake runtests. Optional
                        parameters 'host=', 'tests=[regex], reporter=[list|spec|json|<more>]'.
jake baseline-accept  # This replaces the baseline test results with the results obtained from jake runtests.
jake lint             # Runs tslint on the TypeScript source.
jake help             # List the above commands. 
```


## Usage

```bash
node built/local/tsc.js hello.ts
```


## Roadmap

For details on our planned features and future direction please refer to our [roadmap](https://github.com/Microsoft/TypeScript/wiki/Roadmap).
