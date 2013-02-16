requirejs-mode
==============

requireJS is a Backbone aware minor mode for emacs

The goal is to make writing modules for [requireJS](http://requirejs.org) easier and more
productive.

## Installation

It is available on the [marmalade-repo](http://marmalade-repo.org), so in order to use ELPA:

Make sure you have setup your `packages` to use marmalade.

In your `init.el` file.
```
(require 'package)
(add-to-list 'package-archives 
    '("marmalade" . "http://marmalade-repo.org/packages/"))
(package-initialize)
```

Then, it is as simple as:

```
M-x package-install requirejs-mode
```

Otherwise, make sure you put the `requirejs-mode.el` in your load path.

Then in your `init.el` file.

```
(require 'requirejs-mode)
```

If you would like to automatically load requireJs mode when editing javascript, you can put the
following in your `init.el`. Replace `js2-mode-hook` by `javascript-mode-hook` if you don't use
js2-mode.

```
(add-hook 'js2-mode-hook (lambda () (requirejs-mode)))
```

## Features

### Importing a file as a dependency

When working in a project, it can be error prone to try and remember the exact name and path
of a javascript module you want to import in some other module.

`M-x requirejs-import-file` fixes this problem by giving you a mini-buffer prompt from which 
you can browse your file-system for the said module. It will then insert it in the dependencies
list of your current module definition and insert a CamelCased version of the name of it's
name in the function definition.

It goes the extra mile by doing some processing to the file-name you imported. Here are a 
couple examples. Note: I use [Backbone.js](http://backbonejs.org) lately so most of these
are Backbone related in some way, but not necessarely exclusive to it.

* Importing `.../javascripts/models/item.js`

  Will result in adding `'models/item'` in the dependencies list and `Item` in the function
  definition.

* Importing `.../javascripts/views/item.js`

  Will result in adding `'views/item'` in the dependencies list and `ItemView` in the function
  definition.
  
* Importing `.../javascripts/collections/items/sold-items.js`

  Will result in adding `'collections/items/sold-items'` in the dependencies list and `SoldItems`
  in the function definition.
  
* Importing `.../../templates/item.html`

  Will result in adding `'text!templates/item.html'` in the dependencies list and `ItemTemplate`
  in the function definition.
  
* Importing any file *not* within one of the `{models, views, collections, templates}` sub-folders.

  Will result in adding `'file-name'` in the dependencies list and `FileName` in the function
  definition. Thus assuming you have correctly setup your `require.config({ paths: {...} });`.
  
### Adding a known module as a dependency

When you import a file as a dependency, it will save its `('dependency-name', DependencyDeclaration)`
pair in a temporary associative list for the life-time of your Emacs session. You can then
access these quickly by using `M-x requirejs-import-add` and pick the one you need. It will then
be inserted in your current module definition.

Some defaults are already loaded in this temporary associative list for quick access:

```
'jquery'     -> $
'underscore' -> _
'backbone'   -> Backbone
```

### Creating an empty module

This functionnality is only there to circumvent the need to depend on a snippet system to
create the boilerplate structure of the module. Notice that if the previous features don't
find a proper AMD structure within the current file, it will use this feature to create one
and then insert the desired imports.

Explicity, it generates:
```
define (
    [],    
    function ( ) {
        
    }
);
```

## Keybindings

* `C-c rf` : require-import-file
* `C-c rc` : require-create
* `C-c ra` : require-import-add

## Dependencies

No dependencies, but it will use `ido` to auto-complete prompts if it is present.

## Issues

If you find bugs or you would like new features to be implemented, use the [issue
tracker](https://github.com/ricardmo/requirejs-mode/issues), or better, make a
pull request with the fixes.

## License
Copyright (C) 2013 Marc-Olivier Ricard

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
