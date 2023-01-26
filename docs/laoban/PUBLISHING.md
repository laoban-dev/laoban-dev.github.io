# Publishing to NPM with laoban

# Overview

Laoban can be used to publish to npmjs. This is a very common use case, and laoban has been designed to make this as
easy as possible. If you follow the 'default' project structure all you need to do is to create your NPM_TOKEN
environment variable

This document is for typescript projects, but the same principles apply to javascript projects

## Initial Setup for the first publish

* Ensure that `package.json` has the correct `files` section
* Ensure that `package.json` has the correct `main` section
* Ensure that `package.json` has the correct `types` section
* Enable publishing in the `package.details.json` file
* If you want private publishing you need to modify the `publish script` in the `laoban.json` file
* Add the NPM_TOKEN to the environment

## To actually publish

* Run `laoban update --major` or `laoban update --minor`
* Run `laoban publish`

# The files section

This is provided in the default templates as

```json
{
  "files": [
    "dist/index.js",
    "dist/index.d.ts",
    "dist/src/"
  ]
}
```

It means that `laoban` is expecting a 'index.ts' in the same package as the `package.json` file, and the rest of the
code
in the `src` directory. The `dist` directory is where the compiled code is put.

If you have a different structure you need to make a new template and modify the `files` section. However this is a good
default structure, and you might consider changing to it

# Main and types

Main is the 'entry point' for the package. This is the file that is loaded when you do `require('my-package')`. Types is
the
file that is loaded when you do `import {MyClass} from 'my-package'`. These are both in the `dist` directory.

If you have a different structure you need to make a new template and modify the `main` and `type` section.

```json
{
  "main":  "dist/index",
  "types": "dist/index"
}
```

# Enable publishing in the `package.details.json` file

Each package has a `package.details.json` file. In the guards section there is a `publish` value that is set to false.
In order to publish you need to set this to true

```json
{
  "guards": {
    "publish": true
  }
}
```

# Version numbers

The current version number of all the packages is in the versions.txt file. This is updated by the `laoban update`
command.

```shell
laoban update --major
laoban update --minor
laoban update --setVersion xxx
```

Calling these updates that file, then updates the files in all the projects with the new version number (and any 
template changes)

