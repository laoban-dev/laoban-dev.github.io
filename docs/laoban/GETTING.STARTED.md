# Getting started

After you have [installed](INSTALLING.LAOBAN.md) `laoban` you will need to 
* Set up a `laoban.json`
* add a `package.details.json` file to each subproject
  * Edit for name/description/template and links 
* Create a templates directory 
  * Add a template for each type of project.

# The easy way

Install and use [`laoban-admin`](https://www.npmjs.com/package/@laoban/admin)

# The 'do it yourself way'

It is almost always better to use [`laoban-admin`](https://www.npmjs.com/package/@laoban/admin) but if you want to do it yourself then follow these steps.

## Setting up `laoban.json`
Details of this file can be found [here](LAOBAN.JSON.md). If you are working with typescript and using yarn
a reasonable start is 
```json
{
  "packageManager": "yarn",
  "parents":        [
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/core.laoban.json",
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/typescript.laoban.json",
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/typescript.publish.laoban.json"
  ]}
```

## Setting up `package.details.json`

Each project (a directory with a package.json) needs a `package.details.json` file. This file is used (among other things) to 
describe your project:
* what 'type' of project it is (the template)
* what extra dependancies it has that are not in the template

Example:
```json
{
  "template"   : "typescript",
  "name"       : "laoban",
  "description": "A cli for managing projects that have many npm packages",
  "details"    : {
    "keywords": ["monorepo","devops"],
    "links"       : ["@laoban/variables","@laoban/generations", "@laoban/validation", "@laoban/debug","@laoban/files"],
    "extraDeps"   : {
      "fs-extra" : "^9.0.1",
      "commander": "^6.2.0"
    },
    "extraDevDeps": {"@types/fs-extra": "^9.0.5"},
    "extraBins"   : {"laoban": "dist/index.js"},
    "publish"     : true,
    "tsc": true,
    "test": true
  }
}

```

## Checking it

* `laoban validate` should show you any problems with the `laoban.json` and package.details.json` files
* `laoban packages` should show you all the projects
* `laoban update` will replace the existing package.json files with the ones from the templates
* `yarn` will install the dependancies
* `laoban tsc` will compile the typescript
