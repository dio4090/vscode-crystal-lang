# VSCode Extension for Crystal Language

This extension provides support for The [Crystal](http://crystal-lang.org) programming language.

![crystal icon](http://i.imgur.com/GoiQmzC.gif)

## Features

* Snippets
* Formatting
* Problems finder
* Document Symbols
* Syntax highlighting
* Show variable type on Hover
* Show and Peek Implementations
* Increment and decrements indentation
* Method completion for Literals, Symbols and Top Level Methods

## Configuration

`crystal-lang` provides some useful configuration inside `settings.json`:

```json
{
	"crystal-lang.problems": "syntax",
	"crystal-lang.problemsLimit": 10,
	"crystal-lang.mainFile": "",
  "crystal-lang.processesLimit": 3,
  "crystal-lang.implementations": false,
  "crystal-lang.completion": false,
  "crystal-lang.types": false,

}
```

### problems

`crystal-lang.problems` allow setting different error levels. By default, the problem finder just check syntax errors. The options are:

![problem finder](http://i.imgur.com/ukl1jyg.gif)

- **syntax**: check syntax and tokens (default).
- **build**: check requires, objects and methods without code gen (resource heavy).
- **none**: disable problem finder.

```json
{
	"crystal-lang.problems": "syntax | build | none",
}
```

> Some features like implementations and show type on hover can show their own errors, set "crystal-lang.problems" = "none" to disable them.

### poblemsLimit

`crystal-lang.problemsLimit` allow to limit the amount of problems that appears in problems view. The default value is 20.

### mainFile

`crystal-lang.mainFile` says to the compiler which file should analyze.

It is useful when `"crystal-lang.problems" = "build"` in project where a main file do `require "/**"`

Also is used by features like **implementations** and show **type on hover** to specify the tool scope.

> Be sure that mainFile is a valid **absolute** filepath.

### processesLimit

This extension limit of crystal processes executing in parallel because it doesn't use a language server yet, see [Scry](https://github.com/kofno/scry).

Commonly crystal takes milliseconds to do something like formatting, but in some projects other features like implementations or completion can take a moment. To prevent using too many resources you can set the amount of problems with:

```json
{
	"crystal-lang.processesLimit": 3,
}
```

> By default, is 3. In my computer each crystal process uses almost 50 MB and less than 1 second.

### implementations

You can use this feature to peek or go to implementation of a method. It works per document only, full workspace navigation isn't available yet. However, you can try multiple files implementation setting mainFile in `settings.json`.

### completion

This setting ensure to enable instance method completion using crystal tool context.

Suggestion of methods and subtypes while typing is not supported. You need to type `.` (dot) or `::` (colons) and then press `CTRL + SPACE` or `CMD + SPACE` to call method suggestion.

Basic code completion is always enabled. (Top Level, Symbols and Snippets)

However, you can totally disable completions in `settings.json`:

```json
{
	"editor.quickSuggestions": {
		"other": true,
		"comments": false,
		"strings": false
	}
}
```

### types

Show type information for variables only. This feature uses `crystal tool context` to get types. Information is recalculated when the cursor changes line position.


## ERROR and INFO Messages

> **show types**, **peek implementations** and **complete instance methods** check code errors, if errors exists it waits until they are fixed.

Sometimes in some proyects, `crystal tool` turns heavy, in this case you can check OUTPUT tab in VSCode.

Some errors are:

- `ERROR: crystal formatter --check`: when crystal errors are found in your code.
- `ERROR: crystal build`: when crystal build --no-codegen failed.
- `ERROR: crystal tool context`: when crystal tool found errors in your code.
- `ERROR: spawn`: when crystal program not exist in path or `mainFile` is wrong.
- `ERROR: JSON parse`: when crystal output is different of JSON.
- `INFO: processesLimit has been reached`: your project is taking too much time to analyze.
- `INFO: implementations are disabled`: when `implementations` are disabled in settings.
- `INFO: show types on hover is disabled`: when `types` are disabled in settings.
- `INFO: instance method completion is disabled`: when `completion` is disabled in settings.

## Know issues

- Linter and formatter are implemented using Node.js `child_process`, so perfomance could be affected. You can use a different problem level.

- `macros` can produce some unwanted behaviors, disable _peek implementations, instance method completions and types on hover_ to hide errors.

- ECR syntax is very basic. You can use vscode `text.html` instead or enable emmet for `text.ecr` in your `settings.json`:

```json
{
  "emmet.syntaxProfiles": {
    "ecr": "html"
  }
}
```

- In some big projects like [crystal compiler](http://github.com/crystal-lang) itself, the setting `"crystal-lang.problems" = "build"` could be very unresponsible, use `"syntax"` instead.

- Also, you can disable _peek implementations, instance method completions and types on hover_ to free resources. (**Formatting** and **symbols completion** are **lightweight** features).

## Screenshots

### Increment and decrement identation

![identation](http://i.imgur.com/V15TxFb.gif)

> Decrement `end` keyword on type is now avaliable in vscode insiders [#2262](https://github.com/Microsoft/vscode/issues/2262#issuecomment-309485218).

### Formatting code support

![formatting](http://i.imgur.com/VTeOkOm.gif)

### Syntax highlighting

![ecr](http://i.imgur.com/w9aBlIH.gif)

### Snippets

![snippets](http://i.imgur.com/GNICZSH.gif)

### Code Outline (NEW)

Recent version of VSCode (1.13.1) allow to extensions creators show symbols in tree view. You can use the awesome [Code Outline](https://marketplace.visualstudio.com/items?itemName=patrys.vscode-code-outline) extension to see code tree of crystal document.

### Debugging

[Native Debug](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) is an excelent extension that allow you to debug crystal and other languages that compile to binary.

> Be sure of compile your crystal code with `--debug` flag

![Native Debug extension](http://i.imgur.com/mrJzrxI.png)

### Icon theme

You can use the wonderful [Nomo Dark icon theme](https://marketplace.visualstudio.com/items?itemName=be5invis.vscode-icontheme-nomo-dark) to see crystal icon.

![Nomo Dark icon theme](http://i.imgur.com/6QxIyWV.png)

## Roadmap

- Translate Crystal syntax from `.tmLanguage` to `.json`.
- Support for Language Server Protocol, see [Scry](https://github.com/kofno/scry)]

## Release Notes

See [Changelog](https://github.com/faustinoaq/vscode-crystal-lang/blob/master/CHANGELOG.md)

## Contributing

1. Fork it https://github.com/faustinoaq/vscode-crystal-lang/fork
2. Create your feature branch `git checkout -b my-new-feature`
3. Commit your changes `git commit -am 'Add some feature'`
4. Push to the branch `git push origin my-new-feature`
5. Create a new Pull Request

## Contributors

- [@faustinoaq](https://github.com/faustinoaq) Faustino Aguilar - creator, maintainer