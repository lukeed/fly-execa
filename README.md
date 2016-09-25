# fly-execa [![npm package][npm-ver-link]][npm-pkg-link] [![][travis-badge]][travis-link]
> Execute shell commands with Fly

## Install

```a
npm install --save-dev fly-execa
```

## API

`fly-execa` uses [execa](https://github.com/sindresorhus/execa) as its `child_process` wrapper. This means it has the same options as [child_process.exec](https://nodejs.org/api/child_process.html#child_process_child_process_exec_command_options_callback) and shares `execa`'s [additional options](https://github.com/sindresorhus/execa#options).

### .execa(command, [options])

#### command
Type: `string`<br>
Any occurrences of `$file` will be replaced with the the relevant filepath or glob pattern.

#### options
Type: `object`<br>
See [child_process.exec](https://nodejs.org/api/child_process.html#child_process_child_process_exec_command_options_callback) and [execa](https://github.com/sindresorhus/execa#options) for more info.

#### options.glob
Type: `boolean`<br>
If the command should use the glob pattern within `source()`, you must set this to `true` or `1`. See [here](#iterate-once-per-glob) for example.


## Usage

#### Iterate Once Per File

You can apply a command to each file of your `glob` match. 

Instances of `$file` will be replaced by the file's path.

```js
exports.default = function * () {
  yield this.source('src/*.js')
    .execa('cat $file')
    //=> fly-execa: console.log('this is src/a.js')
    //=> fly-execa: console.log('this is src/b.js')
    //=> fly-execa: console.log('this is src/c.js')
    .dist('dist');
}
```

#### Iterate Once Per Glob

You can use the current glob within your shell command.

> **Note:** Currently, only one glob pattern is supported.

Instances of `$file` will be replaced by the glob:

```js
exports.default = function * () {
  yield this.source('src/*.js')
    .execa('cat $file', {glob: true})
    //=> fly-execa: 
    //=>     console.log('this is src/a.js')
    //=>     console.log('this is src/b.js')
    //=>     console.log('this is src/c.js')
    .dist('dist');
}
```

#### Passing Arguments

Of course, command arguments may be passed within your [command string](#command).

```js
exports.default = function * () {
  yield this.source('src')
    .execa('ls -alh $file', {glob: true})
    .dist('dist');
}
```

## License

MIT © [Luke Edwards](https://lukeed.com)

[npm-pkg-link]: https://www.npmjs.org/package/fly-execa
[npm-ver-link]: https://img.shields.io/npm/v/fly-execa.svg?style=flat-square
[travis-link]:  https://travis-ci.org/lukeed/fly-execa
[travis-badge]: http://img.shields.io/travis/lukeed/fly-execa.svg?style=flat-square
